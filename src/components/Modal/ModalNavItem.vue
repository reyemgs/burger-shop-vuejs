<template>
  <li class="modal-nav-item">{{ item.name }}</li>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
    },
  },
};
</script>

<style lang="scss" src="./modalNavItem.scss"></style>
