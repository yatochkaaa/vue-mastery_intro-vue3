<template>
  <div class="product-display">
    <div class="product-container">
      <div class="product-image">
        <img
          :class="[!inStock ? 'out-of-stock-img' : '']"
          alt="Image"
          :src="image"
        />
      </div>

      <div class="product-info">
        <h1>{{ title }}</h1>
        <p v-if="inStock">In Stock</p>
        <p v-else>Out of Stock</p>
        <p>Shipping: {{ shipping }}</p>
        <product-details :details="details" />
        <div class="color-circle-container">
          <div
            v-for="(variant, index) in variants"
            :key="variant.id"
            @mouseover="updateVariant(index)"
            class="color-circle"
            :style="{ backgroundColor: variant.color }"
          ></div>
        </div>

        <div class="button-container">
          <button
            class="button"
            :class="{ disabledButton: !inStock }"
            @click="addToCart"
            :disabled="!inStock"
          >
            Add to Cart
          </button>

          <button class="button" @click="removeFromCart">
            Remove from Cart
          </button>
        </div>
      </div>
    </div>
    <review-list v-if="reviews.length" :reviews="reviews" />
    <review-form @review-submitted="addReview" />
  </div>
</template>

<script>
import ProductDetails from "./ProductDetails.vue";
import ReviewForm from "./ReviewForm.vue";
import ReviewList from "./ReviewList.vue";

export default {
  props: {
    premium: {
      type: Boolean,
      required: true,
    },
  },
  components: {
    ProductDetails,
    ReviewForm,
    ReviewList,
  },
  data() {
    return {
      product: "Socks",
      brand: "Vue Mastery",
      selectedVariant: 0,
      details: ["50% cotton", "30% wool", "20% polyester"],
      variants: [
        {
          id: 2234,
          color: "green",
          image:
            "https://static.pullandbear.net/2/photos//2023/I/0/2/p/4893/515/800/4893515800_2_1_8.jpg?t=1679483013765&imwidth=1125",
          quantity: 50,
        },
        {
          id: 2235,
          color: "pink",
          image:
            "https://static.nike.com/a/images/t_PDP_1280_v1/f_auto,q_auto:eco/13d5e95b-9dc3-4446-8858-db2ece28b6a2/everyday-plus-cushioned-training-crew-socks-VKWvgn.png",
          quantity: 0,
        },
      ],
      reviews: [],
    };
  },
  methods: {
    addToCart() {
      this.$emit("add-to-cart", this.variants[this.selectedVariant].id);
    },
    removeFromCart() {
      this.$emit("remove-from-cart", this.variants[this.selectedVariant].id);
    },
    updateVariant(index) {
      this.selectedVariant = index;
    },
    addReview(review) {
      this.reviews.push(review);
    },
  },
  computed: {
    title() {
      return this.brand + " " + this.product;
    },
    image() {
      return this.variants[this.selectedVariant].image;
    },
    inStock() {
      return this.variants[this.selectedVariant].quantity;
    },
    shipping() {
      if (this.premium) {
        return "Free";
      }
      return 2.99;
    },
  },
};
</script>

<style scoped>
.product-display {
  display: flex;
  flex-direction: column;
  padding: 1rem;
}

.product-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.product-image,
.product-info {
  width: 50%;
}

@media only screen and (max-width: 600px) {
  .product-image,
  .product-info {
    margin-left: 10px;
    width: 100%;
  }
}
</style>
