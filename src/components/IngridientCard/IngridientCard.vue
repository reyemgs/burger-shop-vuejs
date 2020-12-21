<template>
  <div
    class="ingridient-card"
    :class="{ active: isAdded }"
    @click="addIngridient"
  >
    <img class="ingridient-image" :src="ingridient.image" />
    <span class="ingridient-name">{{ ingridient.name }}</span>
    <span class="ingridient-price">{{ ingridient.price }} &#8381;</span>
  </div>
</template>

<script>
import Vue from 'vue';

export default {
  props: {
    ingridient: {
      type: Object,
    },
    components: {
      type: Object,
    },
  },

  data() {
    return {
      selectedComponents: this.components,
    };
  },

  computed: {
    isAdded() {
      const components = this.selectedComponents[this.ingridient.category];

      if (!Array.isArray(components)) {
        return this.ingridient.key === components;
      } else {
        return components.includes(this.ingridient.key);
      }
    },
  },

  methods: {
    addIngridient() {
      let components = this.selectedComponents[this.ingridient.category];

      if (!Array.isArray(components)) {
        Vue.set(
          this.selectedComponents,
          this.ingridient.category,
          this.ingridient.key
        );
        return;
      } else {
        const addedIngridient = components.find(
          item => item === this.ingridient.key
        );
        if (addedIngridient) {
          this.removeIngridient();
          return;
        }
        components.push(this.ingridient.key);
      }
    },

    removeIngridient() {
      let components = this.components[this.ingridient.category];
      const index = components.findIndex(item => item === this.ingridient.key);
      components.splice(index, 1);
    },
  },
};
</script>

<style lang="scss" src="./ingridientCard.scss"></style>
