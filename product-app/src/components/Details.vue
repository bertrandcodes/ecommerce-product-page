<template>
  <div v-if="!show" class="modal"></div>
  <div v-else class="modal open">
    <div class="detail-image-container">
      <img
        class="detail-image"
        :src="product.previewImage"
        :alt="product.title"
      />
      <Icon name="unfilled" class="unfilled"></Icon>
      <div @click="closeModal" class="cancel-button"></div>
      <Icon name="cancel" class="cancel"> </Icon>
    </div>
    <div class="detail-body">
      <div class="detail-tags">
        <div
          v-for="tag in product.tags"
          :key="tag[0]"
          class="detail-tag"
          :class="getColor"
        >
          {{ tag.toUpperCase() }}
        </div>
      </div>
      <div class="detail-brand">{{ product.brand.toUpperCase() }}</div>
      <div class="detail-title">{{ product.title }}</div>
      <div class="detail-prices">
        <div class="detail-sale">
          {{ currentPrice }}
        </div>
        <div class="detail-original">{{ retailValue }}</div>
        <div class="detail-deal">{{ percentOff }}</div>
      </div>
      <div v-html="product.description" class="detail-description"></div>
      <div class="cart-area">
        <div class="dropdown">
          <button class="quantity-selector">
            <span class="current-quantity">{{ quantity }}</span>
            <Icon v-if="dropdownOpen" @click="toggleQuan" name="up"></Icon>
            <Icon v-else @click="toggleQuan" name="down"></Icon>
          </button>
          <div v-if="dropdownOpen" class="dropdown-content">
            <div @click="changeQuan(1)"><span class="quan-num">1</span></div>
            <div @click="changeQuan(2)"><span class="quan-num">2</span></div>
            <div @click="changeQuan(3)"><span class="quan-num">3</span></div>
            <div @click="changeQuan(4)"><span class="quan-num">4</span></div>
            <div @click="changeQuan(5)"><span class="quan-num">5</span></div>
          </div>
        </div>
        <button id="add-to-cart">Add to Cart</button>
      </div>
      <hr />
      <div onClick="{this.toggleSpecs}" class="specs-box">
        <div class="specs-header">
          <p class="specs">SPECS</p>
          <Icon v-if="showSpecs" @click="toggleSpecs" name="up"></Icon>
          <Icon v-else @click="toggleSpecs" name="down"></Icon>
        </div>
        <div
          v-if="showSpecs"
          v-html="product.specs"
          class="detail-specs open"
        />
        <div v-else v-html="product.specs" class="detail-specs" />
      </div>
      <hr />
    </div>
  </div>
</template>

<script>
import Icon from "./Icon.vue";

export default {
  name: "details-page",
  props: ["product", "show"],
  components: { Icon },
  data() {
    return {
      quantity: 1,
      dropdownOpen: false,
      showSpecs: true,
    };
  },
  methods: {
    closeModal() {
      this.$emit("close-modal");
      console.log("cancel successfully clicked");
    },
    changeQuan(quan) {
      this.quantity = quan;
      this.dropdownOpen = false;
    },
    toggleQuan() {
      this.dropdownOpen = !this.dropdownOpen;
    },
    toggleSpecs() {
      this.showSpecs = !this.showSpecs;
    },
  },
  computed: {
    getColor() {
      const firstTag = this.product.tags[0];
      return {
        blue: firstTag === "Editor's Picks",
        purple: firstTag === "Top Ten",
        pink: firstTag === "Daily Deals",
        red: firstTag === "Olmost Out",
      };
    },
    currentPrice() {
      return `$${this.product.currentPrice.toFixed(2)}`;
    },
    retailValue() {
      return `$${this.product.retailValue.toFixed(2)}`;
    },
    percentOff() {
      return `${Math.round(
        ((this.product.retailValue - this.product.currentPrice) /
          this.product.retailValue) *
          100
      )}% off`;
    },
  },
};
</script>