<template>
  <div id="app">
    <MenuCategoryList :menuCategories="menuCategories"/>
    <br>
    <MenuList :menus="menus"/>
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
    })

  },
  data() {
    return {menus: [], menuCategories: []}
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
