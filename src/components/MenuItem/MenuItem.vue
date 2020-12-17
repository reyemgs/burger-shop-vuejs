<template>
  <li class="menu-item" @click="setCategory">
    {{ category.name }}
  </li>
</template>

<script>
export default {
  props: {
    category: {
      type: Object,
    },
  },
  methods: {
    setCategory() {
      this.$emit('setCategory', this.category.type);
    },
  },
};
</script>

<style></style>
