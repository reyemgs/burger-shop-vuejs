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
    ingridients: {
      type: Object,
    },
    modalIsOpen: {
      type: Boolean,
    },
  },

  // TODO: создать отдельный элемент для модалки
  computed: {
    priceWithIngridients() {
      let price = this.product.price;
      const components = this.product.components;

      for (const category in components) {
        if (!Array.isArray(components[category])) {
          price += this.ingridients[category][components[category]].price;
        } else {
          for (const component of components[category]) {
            price += this.ingridients[category][component].price;
          }
        }
      }
      return price;
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
      if (this.product.type === 'single') {
        this.$emit('addInBasket', {
          id: this.product.id,
          name: this.product.name,
          quantity: this.product.quantity,
          price: this.product.price,
        });
      } else if (this.modalIsOpen) {
        this.$emit('addInBasket', {
          id: this.product.id,
          name: this.product.name,
          quantity: this.product.quantity,
          price: this.priceWithIngridients,
        });
      } else {
        this.$emit('showModal', {
          id: this.product.id,
          name: this.product.name,
          image: this.product.image,
          quantity: this.product.quantity,
          price: this.priceWithIngridients,
          components: this.product.components,
        });
      }
    },
  },
};
</script>

<style lang="scss" src="./scss/addInBasket.scss"></style>
