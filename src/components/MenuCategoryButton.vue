<template>
  <div
      class="col border"
      :class="{
          'bg-warning':selected,
          'bg-light':!selected && mouseHover
        }"
      @click="onClick(menuCategory)"
      @mouseover="mouseHover=true"
      @mouseout="mouseHover=false"
  >
    {{ menuCategory.name }}
  </div>
</template>
<script>
export default {
  name: 'MenuCategoryButton',
  props: {
    menuCategory: Object,
    selected: Boolean,
    onClick: Function
  },
  data() {
    return {
      mouseHover: false
    }
  }
}
</script>