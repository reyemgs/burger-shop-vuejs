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
    <button class="btn-in-basket" @click="addInBasket">
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
    modalIsOpen: {
      type: Boolean,
    },
  },

  methods: {
    increaseQuantity() {
      if (this.product.quantity === 99) return;
      this.product.quantity += 1;
      this.updateQuantity();
    },

    decreaseQuantity() {
      if (this.product.quantity === 1) return;
      this.product.quantity -= 1;
      this.updateQuantity();
    },

    updateQuantity() {
      this.$emit('updateQuantity', {
        id: this.product.id,
        quantity: this.product.quantity,
      });
    },

    addInBasket() {
      if (this.product.type === 'single' || this.modalIsOpen) {
        this.$emit('addInBasket', {
          id: this.product.id,
          name: this.product.name,
          quantity: this.product.quantity,
          price: this.product.price,
        });
      } else {
        this.$emit('showModal', {
          id: this.product.id,
          name: this.product.name,
          quantity: this.product.quantity,
          price: this.product.price,
        });
      }
    },
  },
};
</script>

<style lang="scss" src="./addInBasket.scss"></style>
