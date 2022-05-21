<template>
  <b-modal id="menuModal"
           :title="menu.name"
           @hide="onHide(null)"
           @ok="onOk(menu,1)"
  >
    {{ menu.name + ' ' + menu.price + '원' }}
  </b-modal>
</template>
<script>
export default {
  name: 'MenuModal',
  props: {
    menu: Object,
    onHide: Function,
    onOk: Function
  }
}
</script>
