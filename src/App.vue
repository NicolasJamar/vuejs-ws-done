<script>
import ProductDisplay from "./components/ProductDisplay.vue";
import ReviewForm from "./components/ReviewForm.vue";
import ReviewList from "./components/ReviewList.vue";

export default {
  components: {
    ProductDisplay,
    ReviewForm,
    ReviewList
  },
  data() {
    return{
      cart: 0,
      premium: true,
      reviews: []
    }
  },
  methods: {
    updateCart() {
      this.cart++
    },
    removeFromCart() {
      this.cart--
    },
    addReview(review) {
      this.reviews.push(review)
    }
  }
}
</script>

<template>
  <nav class="nav-bar"></nav>
  
  <ProductDisplay  
    :cart="cart" 
    :premium="premium" 
    @add-to-cart="updateCart" 
    @remove-from-cart="removeFromCart"
    />
  <ReviewList :reviews="reviews" />
  <ReviewForm @review-submitted="addReview" />

</template>

