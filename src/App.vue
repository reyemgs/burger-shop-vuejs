<template>
  <div id="app">
    <Loader v-if="loading" />
    <Header />
    <div class="container">
      <div class="sidebar">
        <MenuList
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
          v-for="product of renderProductsByCategory(currentCategory)"
          :key="product.id"
          :product="product"
          :markets="response.markets"
          @addInBasket="addInBasket"
          @updateQuantity="updateQuantity"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';
import Header from '@/components/Header/Header.vue';
import MenuList from '@/components/MenuList/MenuList.vue';
import Basket from '@/components/Basket/Basket.vue';
import ProductCard from '@/components/ProductCard/ProductCard.vue';
import Loader from '@/components/Loader/Loader.vue';

export default {
  name: 'App',

  components: {
    Loader,
    Header,
    MenuList,
    Basket,
    ProductCard,
  },

  data() {
    return {
      url: './data.json',
      addedProducts: [],
      response: {
        menu: [],
        categories: [],
        modal: [],
        ingridietns: {},
        markets: {},
      },
      loading: true,
      currentCategory: 'pizza',
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
      })
      .catch(error => console.error(error));
  },

  methods: {
    renderProductsByCategory(category) {
      if (this.products.length !== 0) {
        const filteredProducts = this.products.filter(
          product => product.category === category
        );
        return filteredProducts;
      }
    },

    setCategory(category) {
      this.currentCategory = category;
    },

    updateQuantity(product) {
      if (this.addedProducts.length !== 0) {
        const updatedProduct = this.addedProducts.find(
          item => item.id === product.id
        );

        if (updatedProduct !== -1) {
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
  },
};
</script>

<style lang="scss" src="./style/app.scss"></style>
