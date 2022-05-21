<template>
  <div class="container">
    <div class="row row-cols-3">
      <MenuCategoryButton
          v-for="menuCategory in menuCategories"
          :key="menuCategory.id"
          :menuCategory="menuCategory"
          :onClick="onButtonClick"
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
    onButtonClick: Function
  }
}
</script>
