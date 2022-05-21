<template>
  <div id="app">
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
    <b-button class="bg-success">Order(TODO)</b-button>

    <MenuModal
        v-if="selectedMenu"
        :menu="selectedMenu"
        :onHide="setSelectedMenu"
        :on-ok="addCartItem"/>
  </div>
</template>

<script>
import axios from 'axios'
import MenuButtonList from './components/MenuButtonList.vue'
import MenuCategoryButtonList from '@/components/MenuCategoryButtonList'
import MenuModal from '@/components/MenuModal'
import CartItemButtonList from '@/components/CartItemButtonList'

export default {
  name: 'App',
  components: {
    CartItemButtonList,
    MenuModal,
    MenuCategoryButtonList,
    MenuButtonList
  },
  created() {
    Promise.all([
      axios.get('http://localhost:8080/menuCategories'),
      axios.get('http://localhost:8080/menus')
    ]).then(responses => {
      const [menuCategories, menus] = responses
      this.menus = menus.data
      this.menuCategories = menuCategories.data
      this.selectedMenuCategory = this.menuCategories[0]
    })

  },
  data() {
    return {
      menus: [],
      menuCategories: [],
      cartItems: [],
      selectedMenuCategory: null,
      selectedMenu: null,
      selectedCartItem: null
    }
  },
  computed: {
    selectedMenuCategoryMenus() {
      return this.menus.filter(menu => menu.menuCategoryId === this.selectedMenuCategory.id)
    },
    lastCartItemId() {
      if (this.cartItems.length === 0) {
        return -1
      }
      return Math.max(...this.cartItems.map(cartItem => cartItem.id))
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
    setSelectedMenuCategory(menuCategory) {
      this.selectedMenuCategory = menuCategory
    },
    setSelectedMenu(menu) {
      this.selectedMenu = menu
    },
    setSelectedCartMenu(menu) {
      this.selectedCartItem = menu
    },
    addCartItem(menu, qty) {
      this.cartItems.push({
        id: this.lastCartItemId + 1,
        menu: menu,
        qty: qty
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
