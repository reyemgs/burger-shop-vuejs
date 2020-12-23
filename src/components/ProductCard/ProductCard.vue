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
    <button
      class="constructor-btn"
      v-if="product.type === 'multiple'"
      @click="showModal"
    >
      Конструктор
    </button>
    <span class="product-price"
      >&#8381;
      {{
        product.type === 'multiple' ? priceWithIngridients : product.price
      }}</span
    >
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
  </div>
</template>

<script>
export default {
  name: 'ProductCard',
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

  watch: {
    priceWithIngridients: function(val) {
      const price = val;
      this.$emit('updatePrice', price);
    },
  },

  methods: {
    showModal() {
      this.$emit('showModal', {
        id: this.product.id,
        name: this.product.name,
        image: this.product.image,
        quantity: this.product.quantity,
        price:
          this.product.type === 'multiple'
            ? this.priceWithIngridients
            : this.product.price,
        components: this.product.components,
        originalComponents: this.product.originalComponents,
      });
    },

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
      this.$emit('addInBasket', {
        id: this.product.id,
        name: this.product.name,
        quantity: this.product.quantity,
        price:
          this.product.type === 'multiple'
            ? this.priceWithIngridients
            : this.product.price,
      });
    },
  },
};
</script>

<style lang="scss" src="./productCard.scss"></style>
