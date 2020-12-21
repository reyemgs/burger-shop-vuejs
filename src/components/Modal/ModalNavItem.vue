<template>
  <li
    class="modal-nav-item"
    :class="{ active: currentCategory === item.category ? true : false }"
    @click="selectIngridientsCategory"
  >
    {{ item.name }}
  </li>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
    },
    currentCategory: {
      type: String,
    },
  },

  methods: {
    selectIngridientsCategory() {
      this.$emit('selectIngridientsCategory', { category: this.item.category });
    },
  },
};
</script>

<style lang="scss" src="./modalNavItem.scss"></style>
