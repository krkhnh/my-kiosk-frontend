<template>
  <div
      class="col border"
      :class="{'bg-light':mouseHover}"
      @mouseover="mouseHover=true"
      @mouseout="mouseHover=false"
  >
    {{ menu.name + ' ' + menu.price + '원' }}
  </div>
</template>
<script>
export default {
  name: 'MenuButton',
  props: {
    menu: Object
  },
  data() {
    return {
      mouseHover: false
    }
  }
}
</script>
