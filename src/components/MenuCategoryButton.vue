<template>
  <div
      class="col border"
      :class="{
          'bg-warning':selected,
          'bg-light':!selected && mouseHover
        }"
      v-on:click="selectMenuCategory(menuCategory)"
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
    selectMenuCategory: Function
  },
  data() {
    return {
      mouseHover: false
    }
  }
}
</script>