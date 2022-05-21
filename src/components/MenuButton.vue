<template>
  <div
      class="col border"
      :class="{'bg-light':mouseHover}"
      @mouseover="mouseHover=true"
      @mouseout="mouseHover=false"
      v-b-modal:menuModal
      @click="setSelectedMenu(menu)"
  >
    {{ menu.name + ' ' + menu.price + '원' }}
  </div>
</template>
<script>
export default {
  name: 'MenuButton',
  props: {
    menu: Object,
    setSelectedMenu: Function
  },
  data() {
    return {
      mouseHover: false
    }
  }
}
</script>
