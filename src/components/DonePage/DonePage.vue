<template>
  <div class="done-page">
    <img class="product-image" :src="product.image" />
    <div class="done-wrapper">
      <span class="done-header">Ваш сендвич готов!</span>
      <div class="done-info">
        <span class="done-size">{{ sizes }}</span>
        <span class="done-bread">{{ breads }}</span>
        <span class="done-vegetables">{{ vegetables }}</span>
        <span class="done-sauces">{{ sauces }}</span>
        <span class="done-fillings">{{ fillings }}</span>
      </div>
      <span class="done-name">{{ product.name }}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    product: {
      type: Object,
    },
    ingridients: {
      type: Object,
    },
  },

  data() {
    return {
      components: this.product.components,
      allIngridients: this.ingridients,
    };
  },

  computed: {
    sizes() {
      return this.allIngridients.sizes[this.components.sizes].name;
    },

    breads() {
      return this.allIngridients.breads[this.components.breads].name;
    },

    vegetables() {
      const vegetables = [];
      for (const component of this.components.vegetables) {
        vegetables.push(this.allIngridients.vegetables[component].name);
      }
      return vegetables.length !== 0 ? vegetables.join(', ') : 'Нет';
    },

    sauces() {
      const sauces = [];
      for (const component of this.components.sauces) {
        sauces.push(this.allIngridients.sauces[component].name);
      }
      return sauces.length !== 0 ? sauces.join(', ') : 'Нет';
    },

    fillings() {
      const fillings = [];
      for (const component of this.components.fillings) {
        fillings.push(this.allIngridients.fillings[component].name);
      }
      return fillings.length !== 0 ? fillings.join(', ') : 'Нет';
    },
  },
};
</script>

<style lang="scss" src="./donePage.scss"></style>
