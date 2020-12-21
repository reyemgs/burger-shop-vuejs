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
              <IngridientCard
                v-for="(value, property) in ingridientsByCategory"
                :key="ingridientsByCategory[property].id"
                :ingridient="ingridientsByCategory[property]"
                :components="product.components"
              />
            </div>
          </div>

          <div class="modal-footer">
            <span v-if="currentCategory !== 'done'" class="modal-price"
              >{{ product.price }} &#8381;</span
            >
            <div v-else>
              <span class="modal-total-price">{{ totalPrice }} &#8381;</span>
              <AddInBasket
                :product="product"
                :modalIsOpen="modalIsOpen"
                @addInBasket="addInBasket"
                @updateQuantity="updateQuantity"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import ModalNavItem from './ModalNavItem.vue';
import AddInBasket from '../ProductCard/AddInBasket.vue';
import IngridientCard from '../IngridientCard/IngridientCard.vue';

export default {
  components: {
    ModalNavItem,
    AddInBasket,
    IngridientCard,
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
    modalIsOpen: {
      type: Boolean,
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

    updateQuantity(product) {
      this.$emit('updateQuantity', product);
    },

    addInBasket(product) {
      this.$emit('addInBasket', product);
    },
  },
};
</script>

<style lang="scss" src="./modal.scss"></style>
