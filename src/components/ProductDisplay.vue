<script>
import ProductDetails from "./ProductDetails.vue";


export default {
    components: {
        ProductDetails
    },
    props: {
        premium: {
            type: Boolean,
            required: true
        },
        cart: {
            type: Number
        }
    },
    data() {
        return {
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
        }
    },
    methods: {
        addToCart() {
            if(this.variants[this.selectedvariant].quantity > 0) {
                this.$emit("add-to-cart")
                this.variants[this.selectedvariant].quantity--
            }
        },
        removeFromCart() {
            if(this.cart > 0) {
                this.$emit("remove-from-cart")
                this.variants[this.selectedvariant].quantity++
            } 
        },
        updateVariant(index) {
            this.selectedvariant = index
        }
    },
    //Méthodes qui ne sont pas des events
    computed: {
        title() {
            return `${this.brand} ${this.product}`;
        },
        image() {
            return this.variants[this.selectedvariant].image;
        },
        inStock() {
            return this.variants[this.selectedvariant].quantity;
        },
        onSaleDisplay() {
            if (this.onSale) {
                return `${this.brand} ${this.product} is on sale !`;
            }
        },
        shipping() {
            if (this.premium) {
                return "Free"
            } else {
                return 4.99
            }
        }
    }
}
</script>

<template>

    <div class="product-display">
        <div class="product-container">
            <div class="product-image">
                <a :href="url" target="_blank">
                    <img :src="image" :alt="product" :class="{ outOfStockImg: !inStock }">
                </a>
            </div>
            <div class="product-info">
                <h1>{{ title }}</h1>
                <p>{{ description }}</p>
                <p v-if="inStock">In stock</p>
                <p v-else>Out of stock</p>
                <p>{{ onSaleDisplay }}</p>
                <p>Shipping : {{ shipping }}</p>
                <ProductDetails :details="details" />
                <div class="list">
                    <p>Sizes: </p>
                    <ul>
                        <li v-for="size in sizes">
                            {{ size }}
                        </li>
                    </ul>
                </div>
                <div class="circle__container">
                    <div class="color-circle" :style="{ background: variant.color }"
                        v-for="(variant, index) in variants" :key="variant.id" @mouseover="updateVariant(index)">
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

