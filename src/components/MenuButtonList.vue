<template>
  <div class="container">
    <div class="row row-cols-3">
      <MenuButton v-for="menu in menus"
                  :key="menu.id"
                  :menu="menu"
                  :onClick="onButtonClick"
      />
    </div>
  </div>
</template>

<script>
import MenuButton from '@/components/MenuButton'

export default {
  name: 'MenuButtonList',
  components: {MenuButton},
  props: {
    menus: Array,
    onButtonClick: Function
  }
}
</script>

