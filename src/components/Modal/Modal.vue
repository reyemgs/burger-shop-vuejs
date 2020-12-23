<template>
  <transition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="modal-header">
            <span class="modal-title">{{ currentTitle }}</span>
            <div class="modal-close-btn">
              <i
                class="fas fa-times fa-lg"
                aria-hidden="true"
                @click="$emit('closeModal')"
              ></i>
            </div>
          </div>
          <ul class="modal-nav">
            <ModalNavItem
              v-for="item of modalItems"
              :key="item.id"
              :item="item"
              :currentCategory="currentCategory"
              @selectIngridientsCategory="selectIngridientsCategory"
            />
          </ul>
          <div class="modal-body">
            <div class="modal-content">
              <div class="ingridients" v-if="currentCategory != 'done'">
                <IngridientCard
                  v-for="(value, property) in ingridientsByCategory"
                  :key="ingridientsByCategory[property].id"
                  :ingridient="ingridientsByCategory[property]"
                  :components="product.components"
                />
              </div>
              <DonePage v-else :product="product" :ingridients="ingridients" />
            </div>
          </div>

          <div class="modal-footer">
            <span v-if="currentCategory !== 'done'" class="modal-price"
              >{{ product.price }} &#8381;</span
            >
            <div v-else>
              <span class="modal-total-price">{{ totalPrice }} &#8381;</span>
              <div class="modal-add-in-basket">
                <div class="modal-product-quantity-label">Количество</div>
                <div class="modal-btn-decrease">
                  <i
                    class="fas fa-minus-circle"
                    aria-hidden="true"
                    @click="decreaseQuantity"
                  ></i>
                </div>
                <span class="modal-product-quantity">{{
                  product.quantity
                }}</span>
                <div class="modal-btn-increase">
                  <i
                    class="fas fa-plus-circle"
                    aria-hidden="true"
                    @click="increaseQuantity"
                  ></i>
                </div>
                <button class="modal-btn-in-basket" @click="addInBasket">
                  В корзину
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import ModalNavItem from './ModalNavItem.vue';
import IngridientCard from '../IngridientCard/IngridientCard.vue';
import DonePage from '../DonePage/DonePage.vue';

export default {
  components: {
    ModalNavItem,
    IngridientCard,
    DonePage,
  },

  props: {
    modalItems: {
      type: Array,
    },
    product: {
      type: Object,
    },
    ingridients: {
      type: Object,
    },
    price: {
      type: Number,
    },
  },

  data() {
    return {
      currentCategory: 'sizes',
    };
  },

  computed: {
    currentTitle() {
      const foundItem = this.modalItems.find(
        item => item.category === this.currentCategory
      );
      return foundItem.title;
    },

    totalPrice() {
      return this.product.price * this.product.quantity;
    },

    ingridientsByCategory() {
      return this.ingridients[this.currentCategory];
    },
  },

  methods: {
    selectIngridientsCategory(item) {
      if (this.currentCategory === item.category) return;
      this.currentCategory = item.category;
    },

    updateQuantity() {
      this.$emit('updateQuantity', this.product);
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

    addInBasket() {
      this.$emit('addInBasket', this.product);
    },
  },
};
</script>

<style lang="scss" src="./scss/modal.scss"></style>
