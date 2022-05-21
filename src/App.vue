<template>
  <div id="app">
    <MenuCategoryButtonList
        :menuCategories="menuCategories"
        :selectedMenuCategory="selectedMenuCategory"
        :onButtonClick="setSelectedMenuCategory"
    />
    <br>
    <MenuButtonList
        :menus="selectedMenuCategoryMenus"
        :onButtonClick="setSelectedMenu"/>

    <MenuModal
        v-if="selectedMenu"
        :menu="selectedMenu"
        :onHide="setSelectedMenu"/>
  </div>
</template>

<script>
import axios from 'axios'
import MenuButtonList from './components/MenuButtonList.vue'
import MenuCategoryButtonList from '@/components/MenuCategoryButtonList'
import MenuModal from '@/components/MenuModal'

export default {
  name: 'App',
  components: {
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
      selectedMenuCategory: null,
      selectedMenu: null
    }
  },
  computed: {
    selectedMenuCategoryMenus() {
      return this.menus.filter(menu => menu.menuCategoryId === this.selectedMenuCategory.id)
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
