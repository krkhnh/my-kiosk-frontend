<template>
  <div id="app">
    <MenuCategoryList
        :menuCategories="menuCategories"
        :selectedMenuCategory="selectedMenuCategory"
    />
    <br>
    <MenuList :menus="selectedMenuCategoryMenus"/>
  </div>
</template>

<script>
import axios from 'axios'
import MenuList from './components/MenuList.vue'
import MenuCategoryList from '@/MenuCategoryList'

export default {
  name: 'App',
  components: {
    MenuCategoryList,
    MenuList
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
      selectedMenuCategory: null
    }
  },
  computed: {
    selectedMenuCategoryMenus() {
      return this.menus.filter(menu => menu.menuCategoryId === this.selectedMenuCategory.id)
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
