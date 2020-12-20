<template>
  <div id="app">
    <Loader v-if="loading" />
    <Header />
    <div class="container">
      <div class="sidebar">
        <MenuList
          :current-category="currentCategory"
          :categories="response.categories"
          @setCategory="setCategory"
        />
        <Basket
          :added-products="addedProducts"
          @resetQuantity="resetQuantity"
        />
      </div>
      <div class="content">
        <ProductCard
          v-for="product of filteredProducts"
          :key="product.id"
          :product="product"
          :markets="response.markets"
          @addInBasket="addInBasket"
          @updateQuantity="updateQuantity"
          @showModal="openModal"
        />
      </div>
    </div>
    <Modal
      v-if="showModal"
      :product="currentProduct"
      :modalItems="response.modal"
      @closeModal="showModal = false"
    />
  </div>
</template>

<script>
import Vue from 'vue';
import Loader from '@/components/Loader/Loader.vue';
import Header from '@/components/Header/Header.vue';
import MenuList from '@/components/MenuList/MenuList.vue';
import Basket from '@/components/Basket/Basket.vue';
import ProductCard from '@/components/ProductCard/ProductCard.vue';
import Modal from '@/components/Modal/Modal.vue';

export default {
  name: 'App',

  components: {
    Loader,
    Header,
    MenuList,
    Basket,
    ProductCard,
    Modal,
  },

  data() {
    return {
      url: './data.json',
      loading: true,
      showModal: false,
      currentCategory: 'pizza',
      currentProduct: {},
      filteredProducts: [],
      addedProducts: [],
      response: {
        menu: [],
        categories: [],
        modal: [],
        ingridietns: {},
        markets: {},
      },
    };
  },

  computed: {
    products() {
      let id = 1;
      for (const product of this.response.menu) {
        Vue.set(product, 'id', id++);
        Vue.set(product, 'quantity', 1);
      }
      return this.response.menu;
    },
  },

  mounted() {
    fetch(this.url)
      .then(response => response.json())
      .then(json => {
        this.response = json;
        this.loading = false;
        this.renderProductsByCategory(this.currentCategory);
      })
      .catch(error => console.error(error));
  },

  methods: {
    renderProductsByCategory(category) {
      if (this.products.length !== 0) {
        this.filteredProducts = this.products.filter(
          product => product.category === category
        );
      }
    },

    setCategory(category) {
      if (this.currentCategory === category) return;
      this.currentCategory = category;
      this.renderProductsByCategory(category);
    },

    updateQuantity(product) {
      if (this.addedProducts.length !== 0) {
        const updatedProduct = this.addedProducts.find(
          item => item.id === product.id
        );

        if (updatedProduct !== undefined) {
          updatedProduct.quantity = product.quantity;
        }
      }
    },

    resetQuantity(product) {
      const resetedProduct = this.products.find(item => item.id === product.id);
      resetedProduct.quantity = 1;
    },

    addInBasket(product) {
      const addedProduct = this.addedProducts.find(
        item => item.id === product.id
      );
      if (!addedProduct) {
        this.addedProducts.push(product);
      }
    },

    openModal(product) {
      this.showModal = true;
      this.currentProduct = product;
    },
  },
};
</script>

<style lang="scss" src="./style/app.scss"></style>
