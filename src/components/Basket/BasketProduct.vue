<template>
  <div class="basket-product">
    <div class="basket-product-name">{{ product.name }}</div>
    <div class="basket-product-quantity">
      {{ product.quantity }}
    </div>
    <div class="remove-button">
      <i class="fas fa-trash-alt fa-lg" @click="removeProduct"></i>
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
  methods: {
    removeProduct() {
      this.$emit('removeProduct', this.product);
      this.resetQuantity();
    },

    resetQuantity() {
      this.$emit('resetQuantity', this.product);
    },
  },
};
</script>

<style lang="scss" src="./scss/basketProduct.scss"></style>
