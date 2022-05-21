<template>
  <b-modal id="menuModal"
           :title="menu.name"
           @hide="onHide(null)"
           @ok="onOk(menu, qty)"
  >
    {{ menu.name }}
    <br>
    {{ '단가: ' + menu.price + '원' }}
    <br>
    <br>
    <label for="menuQty">수량</label>
    <b-form-spinbutton id="menuQty" min="1" max="10" v-model="qty"/>
    <br>
    <br>
    {{ '합계금액: ' + menu.price * qty + ' 원' }}
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
      qty: 1
    }
  }
}
</script>
