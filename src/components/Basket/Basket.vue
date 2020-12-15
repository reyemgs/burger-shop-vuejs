<template>
  <div class="basket">
    <div class="basket-header">
      <span><i class="fas fa-shopping-basket"></i> Корзина</span>
    </div>

    <div class="basket-body">
      <div class="basket-label-wrapper">
        <span class="basket-name-label">Название</span>
        <span class="basket-quantity-label">Количество</span>
      </div>
      <div class="basket-content-wrapper">
        <span class="empty" v-if="this.addedProducts.length === 0">
          Добавьте товары в корзину
        </span>
        <BasketProduct
          v-else
          v-for="product of addedProducts"
          :key="product.name"
          :product="product"
        />
      </div>
      <span class="basket-total-price">Итого: {{ totalPrice }} &#8381;</span>
      <button class="order-button">Оформить заказ</button>
    </div>
  </div>
</template>

<script>
import BasketProduct from '../BasketProduct/BasketProduct.vue';

export default {
  components: {
    BasketProduct,
  },

  props: {
    product: {
      type: Object,
    },
    addedProducts: {
      type: Array,
    },
  },

  data() {
    return {
      // addedProducts: this.addedProducts,
      totalPrice: 0,
    };
  },

  methods: {
    addProduct(product) {
      this.addedProducts.push(product);
      console.log(this.addedProducts);
    },
  },
};
</script>

<style lang="scss" src="./basket.scss"></style>
