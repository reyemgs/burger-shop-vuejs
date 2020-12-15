<template>
  <div class="add-in-basket">
    <div class="product-quantity-label">Количество</div>
    <div class="btn-decrease">
      <i
        class="fas fa-minus-circle"
        aria-hidden="true"
        @click="decreaseQuantity"
      ></i>
    </div>
    <span class="product-quantity">{{ product.quantity }}</span>
    <div class="btn-increase">
      <i
        class="fas fa-plus-circle"
        aria-hidden="true"
        @click="increaseQuantity"
      ></i>
    </div>
    <button class="btn-in-basket" @click="addInBasket(product)">
      В корзину
    </button>
  </div>
</template>

<script>
export default {
  props: {
    product: {
      type: Object,
      default: null,
      required: true,
    },
  },

  methods: {
    increaseQuantity() {
      if (this.product.quantity === 99) return;
      this.product.quantity += 1;
    },

    decreaseQuantity() {
      if (this.product.quantity === 1) return;
      this.product.quantity -= 1;
    },

    addInBasket(product) {
      this.$emit('addInBasket', {
        name: product.name,
        quantity: product.quantity,
        price: product.price,
      });
    },
  },
};
</script>
