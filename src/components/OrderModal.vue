<template>
  <b-modal
      id="orderModal"
      title="주문"
      @ok="postOrders">
    아래 선택하신 제품을 주문하시겠습니까?
    <br>
    <br>
    <div class="container">
      <div
          class="row row-cols-1"
          v-for="cartItem in cartItems"
          :key="cartItem.id">
        {{ cartItem }}
      </div>
    </div>
    <br>
    <br>
    총 주문금액: {{ finalPrice }}
  </b-modal>
</template>
<script>
import axios from 'axios'

export default {
  name: 'OrderModal',
  props: {
    cartItems: Array,
    onOrdered: Function
  },
  computed: {
    finalPrice() {
      return this.cartItems
          .map(cartItem => cartItem.menu.price * cartItem.quantity)
          .reduce((previousValue, currentValue) => previousValue + currentValue)
    },
    postOrdersResponseBody() {
      return {
        orderItems: this.cartItems.map(cartItem => {
          return {
            menuId: cartItem.menu.id,
            menuPrice: cartItem.menu.price,
            quantity: cartItem.quantity
          }
        })
      }
    }
  },
  data() {
    return {
      quantity: 1
    }
  },
  methods: {
    postOrders() {
      axios.post('http://localhost:8080/orders', this.postOrdersResponseBody
      ).then(() => {
        this.onOrdered()
      })
    }
  }
}
</script>
