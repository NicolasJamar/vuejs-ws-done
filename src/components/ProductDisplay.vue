<script setup>
import { reactive, computed, defineEmits } from 'vue';

import ProductDetails from "./ProductDetails.vue";

const props = defineProps({
    premium: {
        type: Boolean,
        required: true
    },
    cart: {
        type: Number
    }
});

const emit = defineEmits(["add-to-cart", "remove-from-cart"])

const product = reactive({
    product: "Socks",
    brand: "Vue mastery",
    description: "Very nice socks that don't suck",
    url: "https://vuejs.org/guide/introduction.html#api-styles",
    onSale: true,
    details: ["50% coton", "30% wool", "20% polyester"],
    variants: [
        {
            id: 1,
            color: "green",
            image: "./src/assets/images/socks_green.jpg",
            quantity: 15
        },
        {
            id: 2,
            color: "blue",
            image: "./src/assets/images/socks_blue.jpg",
            quantity: 0
        },
    ],
    sizes: ["38-40", "41-43", "44-46"],
    selectedvariant: 0,
});

function addToCart() {
    if(product.variants[product.selectedvariant].quantity > 0) {
        emit("add-to-cart")
        product.variants[product.selectedvariant].quantity--
    }
}
function removeFromCart() {
    console.log(props.cart)
    if(props.cart > 0) {
        emit("remove-from-cart")
        product.variants[product.selectedvariant].quantity++
    } 
}
function updateVariant(index) {
    product.selectedvariant = index
}

//Propriétés qui changent dans le template en fonction du changement des réactives 
const title = computed(() => {
    return `${product.brand} ${product.product}`;
});
const image = computed(() => {
    return product.variants[product.selectedvariant].image;
});
const inStock = computed(() => {
    return product.variants[product.selectedvariant].quantity;
});
const onSaleDisplay = computed(() => {
    if (product.onSale) {
        return `${product.brand} ${product.product} is on sale !`;
    }
});
const shipping = computed(() => {
    if (props.premium) {
        return "Free"
    } else {
        return 4.99
    }
});
    
</script>

<template>

    <div class="product-display">
        <div class="product-container">
            <div class="product-image">
                <a :href="product.url" target="_blank">
                    <img :src="image" :alt="product.product" :class="{ outOfStockImg: !inStock }">
                </a>
            </div>
            <div class="product-info">
                <h1>{{ title }}</h1>
                <p>{{ product.description }}</p>
                <p v-if="inStock">In stock</p>
                <p v-else>Out of stock</p>
                <p>{{ onSaleDisplay }}</p>
                <p>Shipping : {{ shipping }}</p>
                <ProductDetails :details="product.details" />
                <div class="list">
                    <p>Sizes: </p>
                    <ul>
                        <li v-for="size in product.sizes">
                            {{ size }}
                        </li>
                    </ul>
                </div>
                <div class="circle__container">
                    <div class="color-circle" :style="{ background: variant.color }"
                        v-for="(variant, index) in product.variants" :key="variant.id" @mouseover="updateVariant(index)">
                    </div>
                </div>
                <div class="button__container">
                    <button class="button" @click="addToCart" :class="{ disabledButton: !inStock }"
                        :disabled="!inStock">Add to cart</button>
                    <button class="button" @click="removeFromCart" :class="{ disabledButton: cart == 0 }"
                        :disabled="cart == 0">Remove from cart</button>
                </div>
            </div>
            <div class="cart">Cart ({{ cart }})</div>
        </div>
    </div>
</template>

