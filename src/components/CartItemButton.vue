<template>
  <div
      class="col border"
      :class="{'bg-light':mouseHover}"
      @mouseover="mouseHover=true"
      @mouseout="mouseHover=false"
      v-b-modal:menuModal
      @click="onClick(cartItem)"
  >
    {{ cartItem.menu.name + ' x' + cartItem.quantity }}
  </div>
</template>
<script>
export default {
  name: 'CartItemButton',
  props: {
    cartItem: Object,
    onClick: Function
  },
  data() {
    return {
      mouseHover: false
    }
  }
}
</script>
