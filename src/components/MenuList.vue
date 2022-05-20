<template>
  <div class="container border">
    <div class="row row-cols-3">
      <div v-for="menu in menus" v-bind:key="menu.id" class="col border">
        {{ menu.name + ' ' + menu.price + '원' }}
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'MenuList',
  async created() {

    const menus = (await axios.get('http://localhost:8080/menus')).data
    console.log(menus)
    this.menus = menus
  },
  data() {
    return {menus: []}
  }
}
</script>

<style scoped>
</style>
