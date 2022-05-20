<template>
  <div id="app">
    <MenuList/>
  </div>
</template>

<script>
import MenuList from './components/MenuList.vue'

export default {
  name: 'App',
  components: {
    MenuList: MenuList
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
