<template>
  <div class="container">
    <div class="row row-cols-3">
      <div v-for="menu in menus" v-bind:key="menu.id" class="col border">
        {{ menu.name + ' ' + menu.price + '원' }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MenuList',
  props: {
    menus: Array
  }
}
</script>

<style scoped>
</style>
