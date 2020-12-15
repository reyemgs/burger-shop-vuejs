<template>
  <div id="app">
    <Loader v-if="loading" />
    <Header />
    <div class="container">
      <div class="sidebar">
        <MenuList :categories="response.categories" />
        <Basket :added-products="addedProducts" />
      </div>
      <div class="content">
        <ProductCard
          v-for="(product, index) of products"
          :key="index + 1"
          :product="product"
          :markets="response.markets"
          @addInBasket="addInBasket"
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
    addInBasket(product) {
      const addedProduct = this.addedProducts.find(item => item === product);
      if (!addedProduct) {
        this.addedProducts.push(product);
        console.log(this.addedProducts);
      }
    },
  },
};
</script>

<style lang="scss" src="./style/app.scss"></style>
