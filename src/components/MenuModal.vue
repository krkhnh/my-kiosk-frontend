<template>
  <b-modal id="menuModal"
           :title="menu.name"
           @hide="selectMenu(null)"
  >
    {{ menu.name + ' ' + menu.price + '원' }}
  </b-modal>
</template>
<script>
export default {
  name: 'MenuModal',
  props: {
    menu: Object,
    selectMenu: Function
  }
}
</script>
