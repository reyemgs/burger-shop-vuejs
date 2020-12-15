<template>
  <li class="menu-item shadow-pop-br">{{ category.name }}</li>
</template>

<script>
export default {
  props: {
    category: {
      type: Object,
    },
  },
};
</script>

<style></style>
