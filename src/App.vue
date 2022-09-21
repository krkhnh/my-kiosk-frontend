<template>
  <div id="app">
    <b-spinner v-if="!initialized"/>

    <b-container v-if="initialized">
      <b-row cols="1">
        <b-tabs content-class="mt-3">
          <b-tab
              v-for="menuCategory in menuCategories"
              :key="menuCategory.id"
              :title="menuCategory.name"
          >
            <b-container>
              <b-row cols="3" style="height: 200px">
                <b-col
                    v-for="menu in getMenuByCategoryId(menuCategory.id)"
                    :key="menu.id"
                    class="border m-2"
                    @click="setSelectedMenu(menu)"
                >
                  {{ menu.name }}
                </b-col>
              </b-row>
            </b-container>
          </b-tab>
        </b-tabs>
      </b-row>
      <b-row cols="5">
        <b-col
            v-for="cartItem in cartItems"
            :key="cartItem.id"
            class="border m-2"
            @click="setSelectedCartItem(cartItem)"
        >
          {{ cartItem.menu.name + ' ' + cartItem.quantity + ' 개' }}
        </b-col>
      </b-row>
      <b-row cols="2">
        <b-col>
          <b-button
              class="bg-secondary mr-3"
              @click="cartItems.splice(0)">
            Clear
          </b-button>
        </b-col>
        <b-col>
          <b-button
              class="bg-success"
              :disabled="cartItems.length === 0"
              v-b-modal:orderModal>
            Order
          </b-button>
        </b-col>
      </b-row>
    </b-container>

    <MenuModal
        :menu="selectedMenu"
        :onHide="setSelectedMenu"
        :on-ok="addCartItem"/>
    <OrderModal
        v-if="cartItems.length"
        :cart-items="cartItems"
        :onOrdered="resetApp"
    />
  </div>
</template>

<script>
import axios from 'axios';
import MenuModal from '@/components/MenuModal';
import OrderModal from '@/components/OrderModal';

function initialData() {
  return {
    menus: [],
    menuCategories: [],
    cartItems: [],
    selectedMenuCategory: null,
    selectedMenu: null,
    selectedCartItem: null
  };
}

export default {
  name: 'App',
  components: {
    OrderModal,
    MenuModal,
  },
  created() {
    this.getRequiredData();
  },
  data() {
    return {
      menus: [],
      menuCategories: [],
      cartItems: [],
      selectedMenuCategory: null,
      selectedMenu: null,
      selectedCartItem: null
    };
  },
  computed: {
    lastCartItemId() {
      if (this.cartItems.length === 0) {
        return -1;
      }
      return Math.max(...this.cartItems.map(cartItem => cartItem.id));
    },
    initialized() {
      return this.menus.length > 0 && this.menuCategories.length > 0;
    }
  },
  watch: {
    selectedMenu(selectedMenu) {
      if (selectedMenu != null) {
        this.$bvModal.show('menuModal');
      }
    },
  },
  methods: {
    resetApp() {
      Object.assign(this.$data, initialData());
      this.getRequiredData();
    },
    getMenuByCategoryId(cid) {
      return this.menus.filter(menu => menu.menuCategoryIds.includes(cid));
    },
    getRequiredData() {
      Promise.all([
        axios.get('http://localhost:8080/menuCategories'),
        axios.get('http://localhost:8080/menus')
      ]).then(responses => {
        const [menuCategories, menus] = responses;
        this.menus = menus.data;
        this.menuCategories = menuCategories.data;
        this.selectedMenuCategory = this.menuCategories[0];
      }).catch(reason => {
        alert('초기화에 필요한 정보 요청에 실패했습니다. 백엔드 서버가 구동 중 인지 확인해주세요.');
        console.error('요청 실패: ');
        console.error(reason);
      });
    },
    setSelectedMenu(menu) {
      this.selectedMenu = menu;
    },
    setSelectedCartItem(menu) {
      this.selectedCartItem = menu;
    },
    addCartItem(menu, quantity) {
      this.cartItems.push({
        id: this.lastCartItemId + 1,
        menu: menu,
        quantity: quantity
      });
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
