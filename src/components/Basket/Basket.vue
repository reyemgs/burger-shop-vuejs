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
          :key="product.id"
          :product="product"
          @removeProduct="removeProduct"
          @resetQuantity="resetQuantity"
        />
      </div>
      <span class="basket-total-price">Итого: {{ totalPrice }} &#8381;</span>
      <button class="order-button">Оформить заказ</button>
    </div>
  </div>
</template>

<script>
import BasketProduct from './BasketProduct.vue';

export default {
  components: {
    BasketProduct,
  },

  props: {
    addedProducts: {
      type: Array,
    },
  },

  computed: {
    totalPrice() {
      let cartPrice = 0;
      this.addedProducts.forEach(product => {
        cartPrice += product.price * product.quantity;
      });
      return cartPrice;
    },
  },

  methods: {
    removeProduct(product) {
      const index = this.addedProducts.findIndex(item => item === product);
      this.addedProducts.splice(index, 1);
    },

    resetQuantity(product) {
      this.$emit('resetQuantity', product);
    },
  },
};
</script>

<style lang="scss" src="./basket.scss"></style>
