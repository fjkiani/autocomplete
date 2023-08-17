<template>
  <div class="item-container">
    <div class="item-content">
      <div class="item-icon">
        <img :src="item.image" :alt="item.name" width="40" height="40" />
      </div>
      <div class="item-details">
        <div class="item-title">
          <component :is="highlightComponent" :hit="item" attribute="name" />
        </div>
        <div class="item-description">
          By <strong>{{ item.brand }}</strong> in
          <strong>{{ item.categories[0] }}</strong>
        </div>
      </div>
    </div>
    <div class="item-actions">
      <button
        class="item-action-btn select-btn"
        type="button"
        title="Select"
      >
        <!-- SVG for select -->
      </button>
      <button
        class="item-action-btn cart-btn"
        type="button"
        title="Add to cart"
        @click="addToCartHandler"
      >
        <!-- SVG for cart -->
      </button>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  props: {
    item: Object,
    highlightComponent: Function,
  },
  setup() {
    function addToCartHandler(event) {
      event.preventDefault();
      event.stopPropagation();

      console.log('Item added to cart');
    }

    return {
      addToCartHandler,
    };
  },
};
</script>
