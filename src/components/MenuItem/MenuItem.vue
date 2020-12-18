<template>
  <li
    class="menu-item"
    @click="setCategory"
    :class="{ active: currentCategory === category.type ? true : false }"
  >
    {{ category.name }}
  </li>
</template>

<script>
export default {
  props: {
    category: {
      type: Object,
    },
    currentCategory: {
      type: String,
    },
  },

  data() {
    return {
      active: false,
    };
  },

  methods: {
    setCategory() {
      this.$emit('setCategory', this.category.type);
    },
  },
};
</script>

<style></style>
