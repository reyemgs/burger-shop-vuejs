<template>
  <transition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="modal-header">
            <span class="modal-title">{{ modalItems[0].title }}</span>
            <div class="modal-close-btn">
              <i
                class="fas fa-times"
                aria-hidden="true"
                @click="$emit('closeModal')"
              ></i>
            </div>
          </div>

          <div class="modal-body">
            <ul class="modal-nav">
              <ModalNavItem
                v-for="item of modalItems"
                :key="item.id"
                :item="item"
              />
            </ul>
            <div class="modal-content"></div>
          </div>

          <div class="modal-footer">&#8381; {{ product.price }}</div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import ModalNavItem from './ModalNavItem.vue';

export default {
  components: {
    ModalNavItem,
  },

  props: {
    modalItems: {
      type: Array,
    },
    product: {
      type: Object,
    },
  },
};
</script>

<style lang="scss" src="./modal.scss"></style>
