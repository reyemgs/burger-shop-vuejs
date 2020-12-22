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
    <AddInBasket
      :product="product"
      :ingridients="ingridients"
      @addInBasket="addInBasket"
      @showModal="showModal"
      @updateQuantity="updateQuantity"
    />
  </div>
</template>

<script>
import AddInBasket from './AddInBasket.vue';

export default {
  props: {
    product: {
      type: Object,
      default: null,
      required: true,
    },
    ingridients: {
      type: Object,
    },
    markets: {
      type: Object,
      default: null,
    },
  },

  components: {
    AddInBasket,
  },

  computed: {
    // priceWithIngridients() {
    //   let price = this.product.price;
    //   const components = this.product.components;
    //   for (const category in components) {
    //     if (!Array.isArray(components[category])) {
    //       price += this.ingridients[category][components[category]].price;
    //     } else {
    //       for (const component of components[category]) {
    //         price += this.ingridients[category][component].price;
    //       }
    //     }
    //   }
    //   return price;
    // },
  },

  methods: {
    updateQuantity(product) {
      this.$emit('updateQuantity', product);
    },

    addInBasket(product) {
      this.$emit('addInBasket', product);
    },

    showModal(product) {
      this.$emit('showModal', product);
    },
  },
};
</script>

<style lang="scss" src="./scss/productCard.scss"></style>
