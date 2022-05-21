<template>
  <div class="container">
    <div class="row row-cols-3">
      <div v-for="menuCategory in menuCategories" v-bind:key="menuCategory.id" class="col border">
        {{ menuCategory.name }}
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'MenuCategoryList',
  props: {
    menuCategories: Array
  }
}
</script>
