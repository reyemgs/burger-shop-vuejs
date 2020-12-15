<template>
  <div class="basket-product">
    <div class="basket-product-name">{{ product.name }}</div>
    <div class="basket-product-quantity">{{ product.quantity }}</div>
    <div class="remove-button">
      <i class="fas fa-trash-alt fa-lg"></i>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    product: {
      type: Object,
    },
  },
};
</script>

<style></style>
