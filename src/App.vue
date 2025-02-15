<template>
  <div class="app">
    <!-- Navigation Bar -->
    <nav class="nav">
      <div class="logo">Unknown Scents</div>
      <div class="nav-links">
        <a href="#home">Home</a>
        <a href="#shop">Shop</a>
        <a href="#bestsellers">Best Sellers</a>
        <a href="#contact">Contact</a>
      </div>
    </nav>

    <!-- Hero Banner -->
    <div class="hero">
      <h1>Unknown Scents</h1>
      <p>Discover Shi Bru</p>
    </div>

    <!-- Featured Products -->
    <div class="featured">
      <h2>Best Sellers</h2>
      <div class="product-grid">
        <div v-for="perfume in perfumes" :key="perfume.id" class="product-card">
          <img :src="perfume.image" :alt="perfume.name">
          <h3>{{ perfume.name }}</h3>
          <p>{{ perfume.description }}</p>
          <p class="price">${{ perfume.price }}</p>
          <button @click="addToCart(perfume)">Add to Cart</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import 'primeicons/primeicons.css'
// Your perfume data
const perfumes = ref([
  {
    id: 1,
    name: 'Cyber Byte',
    description: 'A gulp of air from the Y2K aesthetic',
    price: 79.99,
    image: '/perfume1.jpg'
  },
  {
    id: 2,
    name: 'Digital Diva',
    description: 'A Vocaloid Inspired Scent',
    price: 89.99,
    image: '/perfume2.jpg'
  },
  // Add more perfumes here
])

// Add to cart function (we'll expand this later)
const addToCart = (perfume: any) => {
  alert(`Added ${perfume.name} to cart!`)
}
</script>

<style>
.app {
  font-family: Arial, sans-serif;
  max-width: 1200px;
  margin: 0 auto;
}

.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  border-bottom: 1px solid #eee;
}

.nav-links a {
  margin-left: 2rem;
  text-decoration: none;
  color: #333;
}

.hero {
  text-align: center;
  padding: 4rem 2rem;
  background: #f9f9f9;
  margin: 2rem 0;
}

.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
  padding: 2rem 0;
}

.product-card {
  border: 1px solid #eee;
  padding: 1rem;
  text-align: center;
  border-radius: 8px;
}

.product-card img {
  max-width: 100%;
  height: auto;
}

.price {
  font-weight: bold;
  font-size: 1.2rem;
  color: #2c3e50;
}

button {
  background: #000;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background: #333;
}
</style>