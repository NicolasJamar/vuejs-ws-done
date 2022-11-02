<script setup>
import { ref } from "vue"; 
import ProductDisplay from "./components/ProductDisplay.vue";
import ReviewForm from "./components/ReviewForm.vue";
import ReviewList from "./components/ReviewList.vue";


let cart = ref(0)
let premium = ref(true)
const reviews = ref([])

function updateCart() {
  cart.value++
}
function removeFromCart() {
  cart.value--
}
function addReview(review) {
  reviews.value.push(review)
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

