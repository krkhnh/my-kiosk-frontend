<template>
  <div id="app">
    <MenuCategoryList
        :menuCategories="menuCategories"
        :selectedMenuCategory="selectedMenuCategory"
        :selectMenuCategory="selectMenuCategory"
    />
    <br>
    <MenuButtonList :menus="selectedMenuCategoryMenus"/>
  </div>
</template>

<script>
import axios from 'axios'
import MenuButtonList from './components/MenuButtonList.vue'
import MenuCategoryList from '@/components/MenuCategoryList'

export default {
  name: 'App',
  components: {
    MenuCategoryList,
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
      selectedMenuCategory: null
    }
  },
  computed: {
    selectedMenuCategoryMenus() {
      return this.menus.filter(menu => menu.menuCategoryId === this.selectedMenuCategory.id)
    }
  },
  methods: {
    selectMenuCategory(menuCategory) {
      this.selectedMenuCategory = menuCategory
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
