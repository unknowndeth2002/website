<script setup lang="ts">
import { ref, computed } from 'vue'
import 'primeicons/primeicons.css'

//always try to keep code at the top and styles / css stuff down all code goes into the scirpt tag
type Perfume = {
  id: number
  name: string
  stock?: number
  category?: string
  price: number
  image: string
  description: string
}

const cartItems = ref<Perfume[]>([])
const isCartVisible = ref(false)

const toggleCart = () => {
  console.log('Current visibility:', isCartVisible.value)
  isCartVisible.value = !isCartVisible.value
  console.log('New visibility:', isCartVisible.value)
}

// Enhanced addToCart function
const addToCart = (perfume: Perfume) => {
  cartItems.value.push(perfume);
}

// Calculate cart total
const calculateTotal = (): number => {
  return parseFloat(cartItems.value.reduce((total, item) => total + item.price, 0).toFixed(2))
}

// Sample perfume data
const perfumes = ref<Perfume[]>([
  {
    id: 1,
    name: 'Summer Breeze',
    description: 'Light and fresh with citrus notes',
    price: 79.99,
    image: '/images/perfumes/summer-breeze.jpg',
    stock: 10,
    category: 'Fresh'
  },
  {
    id: 2,
    name: 'Midnight Rose',
    description: 'Elegant rose with vanilla undertones',
    price: 89.99,
    image: '/images/perfumes/midnight-rose.jpg',
    stock: 5,
    category: 'Floral'
  },
  // Copy and paste the above format to add more perfumes!
]);

// Add a ref for the selected category
const selectedCategory = ref<string>('All')

// Filter perfumes by category using a computed property
const filteredPerfumes = computed((): Perfume[] => {
  if (selectedCategory.value === 'All') {
    return perfumes.value
  } else {
    return perfumes.value.filter(perfume => perfume.category === selectedCategory.value);
  }
});

// Update the filterByCategory function
const filterByCategory = (category: string): void => {
  selectedCategory.value = category
}
</script>

<template>
  <div class="app">
    <nav class="nav">
      <div class="logo">
        <a>[LOGOPLACEHOLDER]</a>
      </div>
      <div class="nav-links">
        <a href="#home">Home</a>
        <a href="#shop">Shop</a>
        <a href="#bestsellers">Best Sellers</a>
        <a href="#contact">Contact</a>
      </div>
      <div class="cart-icon" @click="toggleCart">
        <i class="pi pi-shopping-cart"></i>
        <span class="cart-count">{{ cartItems.length }}</span>
      </div>
    </nav>
    <div class="shopping-cart" v-if="isCartVisible">
      <div class="cart-header">
        <h3>Your Cart</h3>
        <button @click="toggleCart">Close</button>
      </div>
      <div v-if="cartItems.length === 0" class="empty-cart">
        <p>Your cart is empty</p>
      </div>
      <div v-else class="cart-items">
        <div v-for="item in cartItems" :key="item.id" class="cart-item">
          <img :src="item.image" :alt="item.name">
          <h4>{{ item.name }}</h4>
          <p>${{ item.price }}</p>
        </div>
      </div>

      <div class="cart-total">
        Total: ${{ calculateTotal() }}
      </div>
    </div>

    <div class="hero">
      <h1>Luxury Fragrances</h1>
      <p>Discover your signature scent</p>
    </div>

    <div class="filters">
      <button v-for="category in ['All', 'Fresh', 'Floral', 'Woody']" :key="category"
        @click="filterByCategory(category)">
        {{ category }}
      </button>
    </div>

    <!-- Product display section -->
    <div class="featured">
      <h2>Best Sellers</h2>
      <div class="product-grid">
        <div v-for="perfume in filteredPerfumes" :key="perfume.id" class="product-card">
          <img :src="perfume.image" :alt="perfume.name">
          <h3>{{ perfume.name }}</h3>
          <p>{{ perfume.description }}</p>
          <p class="price">${{ perfume.price }}</p>
          <!-- Add stock indicator -->
          <p class="stock" :class="{ 'low-stock': (perfume.stock || 0) < 5 }">
            In Stock: {{ perfume.stock || 0 }}
          </p>
          <button @click="addToCart(perfume)" :disabled="(perfume.stock || 0) === 0">
            Add to Cart
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.app {
  /* 👇 CHANGE FONT if you want a different one */
  font-family: Arial, sans-serif;
  max-width: 1200px;
  margin: 0 auto;
}

/* Navigation bar styles */
.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: white;
  border-bottom: 1px solid black;
  position: relative;
}

/* Navigation link styles */
.nav-links a {
  margin-left: 3.5rem;
  text-decoration: none;
  color: white;
}

/* Hero section styles */
.hero {
  text-align: center;
  padding: 4rem 2rem;
  background: #030910;
  margin: 2rem 0;
  color: white;
  border-radius: 70%;
  border: black;
}


/*shopping cart display*/
.shopping-cart {
  position: fixed;
  top: 0;
  right: 0;
  width: 300px;
  height: 100vh;
  background: #030910;
  box-shadow: -2px 0 5px rgba(0, 0, 0, 0.2);
  padding: 1rem;
  z-index: 9999;
  border: 1px solid black;
  pointer-events: all;
  overflow-y: auto;
  transform: translateZ(0);
}

.cart-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  border-bottom: black;
}

.cart-items {
  max-height: calc(100vh - 150px);
  overflow-y: auto;
}

.cart-item {
  display: flex;
  gap: 1rem;
  padding: 1rem 0;
  border-bottom: 1px solid #eee;
}

.empty-cart {
  text-align: center;
  padding: 2rem;
  color: #eee;
}

.cart-total {
  text-align: right;
  margin-top: 1rem;
  font-weight: bold;
}

/* Product grid layout */
.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
  padding: 2rem 0;
}

/* Individual product card styles */
/* CHANGE COLORS of the card borders and background */
.product-card {
  border: 1px solid black;
  color: white;
  background-color: #030910;
  padding: 1rem;
  text-align: center;
  border-radius: 8px;
}

/* Product image styles */
.product-card img {
  max-width: 100%;
  height: auto;
}

/* Price text styles */
/* CHANGE COLOR of the price text */
.price {
  font-weight: bold;
  font-size: 1.2rem;
  color: lightskyblue;
}

/* Button styles */
button {
  background: white;
  color: black;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
}

/* Button hover effect */
button:hover {
  background: darkgray;
}

/* Add new styles */
.cart-icon {
  position: relative;
  padding: 1rem;
  cursor: pointer;
  margin-left: auto;
}

.cart-count {
  position: absolute;
  top: 0;
  right: 0;
  background: lightcoral;
  color: white;
  border-radius: 50%;
  padding: 0.2rem 0.5rem;
  font-size: 0.6rem;
}

.filters {
  display: flex;
  gap: 1rem;
  padding: 1rem;
  justify-content: center;
}

.filters button {
  padding: 0.5rem 1rem;
  border-radius: 4px;
  background: white;
  cursor: pointer;
}

.filters button:hover {
  background: darkgray;
}

.stock {
  color: lightcyan;
  font-size: 0.9rem;
}

.low-stock {
  color: #ff4444;
}

/* Make the site responsive */
@media (max-width: 768px) {
  .product-grid {
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  }

  .nav-links {
    display: none;
    /* Consider adding a mobile menu */
  }
}
</style>
