<template>
  <b-modal id="menuModal"
           :title="menu.name"
           @hide="onHide(null)"
           @ok="onOk(menu, quantity)"
  >
    {{ menu.name }}
    <br>
    {{ '단가: ' + menu.price + '원' }}
    <br>
    <br>
    <label for="menuQuantity">수량</label>
    <b-form-spinbutton id="menuQuantity" min="1" max="10" v-model="quantity"/>
    <br>
    <br>
    {{ '합계금액: ' + menu.price * quantity + ' 원' }}
  </b-modal>
</template>
<script>
export default {
  name: 'MenuModal',
  props: {
    menu: Object,
    onHide: Function,
    onOk: Function
  },
  data() {
    return {
      quantity: 1
    }
  }
}
</script>
