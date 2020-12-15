<template>
  <ul class="menu-list">
    <MenuItem
      v-for="category of categories"
      :category="category"
      :key="category.type"
    />
  </ul>
</template>

<script>
import MenuItem from '../MenuItem/MenuItem.vue';

export default {
  props: {
    categories: {
      type: Array,
    },
  },

  components: {
    MenuItem,
  },
};
</script>

<style lang="scss" src="./menuList.scss"></style>
