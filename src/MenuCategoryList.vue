<template>
  <div class="container">
    <div class="row row-cols-3">
      <div v-for="menuCategory in menuCategories"
           v-bind:key="menuCategory.id"
           class="col border"
           :class="{'bg-warning':menuCategory===selectedMenuCategory}"
           v-on:click="selectMenuCategory(menuCategory)"
      >
        {{ menuCategory.name }}
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'MenuCategoryList',
  props: {
    menuCategories: Array,
    selectedMenuCategory: Object,
    selectMenuCategory: Function
  }
}
</script>
