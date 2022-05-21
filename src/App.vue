<template>
  <div id="app">
    <b-spinner v-if="!initialized"/>

    <template v-if="initialized">
      <MenuCategoryButtonList
          :menuCategories="menuCategories"
          :selectedMenuCategory="selectedMenuCategory"
          :onButtonClick="setSelectedMenuCategory"/>
      <br>
      <MenuButtonList
          :menus="selectedMenuCategoryMenus"
          :onButtonClick="setSelectedMenu"/>
      <br>
      <CartItemButtonList
          :cartItems="cartItems"
          :onButtonClick="setSelectedCartMenu"/>
      <br>
      <b-button
          class="bg-secondary mr-3"
          @click="cartItems.splice(0)">
        Clear
      </b-button>
      <b-button
          class="bg-success"
          :disabled="cartItems.length === 0"
          v-b-modal:orderModal>
        Order
      </b-button>

      <MenuModal
          v-if="selectedMenu"
          :menu="selectedMenu"
          :onHide="setSelectedMenu"
          :on-ok="addCartItem"/>
      <OrderModal
          v-if="cartItems.length"
          :cart-items="cartItems"
          :onOrdered="resetComponent"
      />
    </template>
  </div>
</template>

<script>
import axios from 'axios'
import MenuButtonList from './components/MenuButtonList.vue'
import MenuCategoryButtonList from '@/components/MenuCategoryButtonList'
import MenuModal from '@/components/MenuModal'
import CartItemButtonList from '@/components/CartItemButtonList'
import OrderModal from '@/components/OrderModal'

function initialData() {
  return {
    menus: [],
    menuCategories: [],
    cartItems: [],
    selectedMenuCategory: null,
    selectedMenu: null,
    selectedCartItem: null
  }
}

export default {
  name: 'App',
  components: {
    OrderModal,
    CartItemButtonList,
    MenuModal,
    MenuCategoryButtonList,
    MenuButtonList
  },
  created() {
    this.getRequiredData()
  },
  data() {
    return initialData()
  },
  computed: {
    selectedMenuCategoryMenus() {
      return this.menus.filter(menu => menu.menuCategoryIds.includes(this.selectedMenuCategory.id))
    },
    lastCartItemId() {
      if (this.cartItems.length === 0) {
        return -1
      }
      return Math.max(...this.cartItems.map(cartItem => cartItem.id))
    },
    initialized() {
      return this.menus.length > 0 && this.menuCategories.length > 0
    }
  },
  watch: {
    selectedMenu(selectedMenu) {
      if (selectedMenu !== null) {
        this.$bvModal.show('menuModal')
      }
    },
  },
  methods: {
    resetComponent() {
      Object.assign(this.$data, initialData())
      this.getRequiredData()
    },
    getRequiredData() {
      Promise.all([
        axios.get('http://localhost:8080/menuCategories'),
        axios.get('http://localhost:8080/menus')
      ]).then(responses => {
        const [menuCategories, menus] = responses
        this.menus = menus.data
        this.menuCategories = menuCategories.data
        this.selectedMenuCategory = this.menuCategories[0]
      }).catch(reason => {
        alert('초기화에 필요한 정보 요청에 실패했습니다. 백엔드 서버가 구동 중 인지 확인해주세요.')
        console.error('요청 실패: ')
        console.error(reason)
      })
    },
    setSelectedMenuCategory(menuCategory) {
      this.selectedMenuCategory = menuCategory
    },
    setSelectedMenu(menu) {
      this.selectedMenu = menu
    },
    setSelectedCartMenu(menu) {
      this.selectedCartItem = menu
    },
    addCartItem(menu, quantity) {
      this.cartItems.push({
        id: this.lastCartItemId + 1,
        menu: menu,
        quantity: quantity
      })
    }
  }
}
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
