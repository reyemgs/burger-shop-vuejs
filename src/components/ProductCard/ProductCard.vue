<template>
  <div class="product-card">
    <div class="product-header">
      <img :src="markets[product.market].image" class="product-market" />
      <img :src="product.image" class="product-image" />
    </div>
    <div class="product-name">
      {{ product.name }}
    </div>
    <p class="product-description">
      {{ product.description }}
    </p>
    <span class="product-price">&#8381; {{ product.price }}</span>
    <AddInBasket :product="product" @addInBasket="addInBasket" />
  </div>
</template>

<script>
import AddInBasket from '../AddInBasket/AddInBasket.vue';

export default {
  props: {
    product: {
      type: Object,
      default: null,
      required: true,
    },
    markets: {
      type: Object,
      default: null,
    },
  },

  components: {
    AddInBasket,
  },

  methods: {
    addInBasket(product) {
      this.$emit('addInBasket', product);
    },
  },
};
</script>

<style lang="scss" src="./productCard.scss"></style>
