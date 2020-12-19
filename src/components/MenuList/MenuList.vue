<template>
  <ul class="menu-list">
    <MenuItem
      v-for="category of categories"
      :current-category="currentCategory"
      :category="category"
      :key="category.type"
      @setCategory="setCategory"
    />
  </ul>
</template>

<script>
import MenuItem from './MenuItem.vue';

export default {
  props: {
    categories: {
      type: Array,
    },
    currentCategory: {
      type: String,
    },
  },

  components: {
    MenuItem,
  },

  methods: {
    setCategory(category) {
      this.$emit('setCategory', category);
    },
  },
};
</script>

<style lang="scss" src="./menuList.scss"></style>
