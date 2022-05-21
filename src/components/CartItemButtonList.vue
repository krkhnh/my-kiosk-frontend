<template>
  <div class="container">
    <div class="row row-cols-3">
      <CartItemButton v-for="cartItem in cartItems"
                      :key="cartItem.id"
                      :cartItem="cartItem"
                      :onClick="onButtonClick"
      />
    </div>
  </div>
</template>

<script>
import CartItemButton from '@/components/CartItemButton'

export default {
  name: 'CartItemButtonList',
  components: {CartItemButton},
  props: {
    cartItems: Array,
    onButtonClick: Function
  }
}
</script>

