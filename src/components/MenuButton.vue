<template>
  <div
      class="col border"
      :class="{'bg-light':mouseHover}"
      @mouseover="mouseHover=true"
      @mouseout="mouseHover=false"
      v-b-modal:menuModal
      @click="onClick(menu)"
  >
    {{ menu.name + ' ' + menu.price + '원' }}
  </div>
</template>
<script>
export default {
  name: 'MenuButton',
  props: {
    menu: Object,
    onClick: Function
  },
  data() {
    return {
      mouseHover: false
    }
  }
}
</script>
