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
          @removeProduct="removeProduct"
          @resetQuantity="resetQuantity"
        />
      </div>
      <div class="content">
        <ProductCard
          v-for="product of filteredProducts"
          :key="product.id"
          :product="product"
          :ingridients="ingridients"
          :markets="response.markets"
          @addInBasket="addInBasket"
          @updateQuantity="updateQuantity"
          @updatePrice="updatePrice"
          @showModal="openModal"
        />
      </div>
    </div>
    <Modal
      v-if="showModal"
      :product="currentProduct"
      :ingridients="ingridients"
      :modalItems="response.modal"
      @closeModal="closeModal"
      @addInBasket="addInBasket"
      @updateQuantity="updateQuantity"
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
import { cloneDeep } from 'lodash';

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
      currentCategory: 'sandwiches',
      currentProduct: {},
      filteredProducts: [],
      addedProducts: [],
      response: {
        menu: [],
        categories: [],
        modal: [],
        ingridients: {},
        markets: {},
      },
    };
  },

  computed: {
    products() {
      let products = this.response.menu;
      let id = 1;
      for (const product of products) {
        Vue.set(product, 'id', id++);
        Vue.set(product, 'quantity', 1);
      }
      return products;
    },

    ingridients() {
      let ingridients = this.response.ingridients;
      let id = 1;
      for (const key in ingridients) {
        for (const ingridient in ingridients[key]) {
          Vue.set(ingridients[key][ingridient], 'id', id++);
          Vue.set(ingridients[key][ingridient], 'key', ingridient);
          Vue.set(ingridients[key][ingridient], 'category', key);
        }
      }
      return ingridients;
    },
  },

  async mounted() {
    const response = await fetch(this.url)
      .then(response => {
        return response.json();
      })
      .then(json => {
        return json;
      })
      .catch(error => console.error(error));
    this.response = {
      ...response,
      products: response.menu.map(product => {
        Vue.set(product, 'originalComponents', cloneDeep(product.components));
      }),
    };
    this.loading = false;
    this.renderProductsByCategory(this.currentCategory);
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
      if (this.showModal) {
        this.updateModalQuantity(product);
      }

      if (this.addedProducts.length !== 0) {
        const updatedProduct = this.addedProducts.find(
          item => item.id === product.id
        );

        if (updatedProduct !== undefined) {
          updatedProduct.quantity = product.quantity;
        }
      }
    },

    removeProduct(product) {
      const index = this.addedProducts.findIndex(item => item === product);
      const foundProduct = this.products.find(item => item.id === product.id);
      this.addedProducts.splice(index, 1);
      if (foundProduct.components) {
        foundProduct.components = cloneDeep(foundProduct.originalComponents);
      }
    },

    resetQuantity(product) {
      const resetedProduct = this.products.find(item => item.id === product.id);
      resetedProduct.quantity = 1;
    },

    updateModalQuantity(product) {
      const updatedProduct = this.products.find(item => item.id === product.id);
      updatedProduct.quantity = product.quantity;
    },

    updatePrice(price) {
      this.currentProduct.price = price;
      const foundProduct = this.addedProducts.find(
        item => item.id === this.currentProduct.id
      );
      if (foundProduct) {
        foundProduct.price = price;
      }
    },

    addInBasket(product) {
      const addedProduct = this.addedProducts.find(
        item => item.id === product.id
      );
      if (!addedProduct) {
        this.addedProducts.push(product);
      }
      if (this.showModal) this.closeModal();
    },

    openModal(product) {
      this.showModal = true;
      this.currentProduct = product;
      document.querySelector('body').style.overflow = 'hidden';
    },

    closeModal() {
      this.showModal = false;
      document.querySelector('body').removeAttribute('style');
    },
  },
};
</script>

<style lang="scss" src="./style/app.scss"></style>
