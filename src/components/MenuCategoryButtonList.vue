<template>
  <div class="container">
    <div class="row row-cols-3">
      <MenuCategoryButton
          v-for="menuCategory in menuCategories"
          v-bind:key="menuCategory.id"
          :menuCategory="menuCategory"
          :select-menu-category="selectMenuCategory"
          :selected="menuCategory===selectedMenuCategory"
      />
    </div>
  </div>
</template>
<script>
import MenuCategoryButton from '@/components/MenuCategoryButton'

export default {
  name: 'MenuCategoryButtonList',
  components: {MenuCategoryButton},
  props: {
    menuCategories: Array,
    selectedMenuCategory: Object,
    selectMenuCategory: Function
  }
}
</script>
