<template>
    <!-- This is the main container of your website -->
    <div class="app">
      <div class="cart-icon" @click="toggleCart">
  <i class="pi pi-shopping-cart"></i> 
  <span class="cart-count">{{ cartItems.length }}</span>
</div>

<div class="shopping-cart" v-show="isCartVisible">
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

      <nav class="nav">
        <!-- 👇 CHANGE THIS TEXT to your store name -->
        <div class="logo">Your Brand Name</div>
        <!-- These are your menu links -->
        <div class="nav-links">
          <a href="#home">Home</a>
          <a href="#shop">Shop</a>
          <a href="#bestsellers">Best Sellers</a>
          <a href="#contact">Contact</a>
        </div>
      </nav>
  

      <div class="hero">
        <h1>Luxury Fragrances</h1>
        <p>Discover your signature scent</p>
      </div>
      <div class="nav-links">
        <a href="#home"><i class="pi pi-home"></i> Home</a>
        <a href="#shop"><i class="pi pi-shopping-bag"></i> Shop</a>
        <a href="#bestsellers"><i class="pi pi-star"></i> Best Sellers</a>
        <a href="#contact"><i class="pi pi-envelope"></i> Contact</a>
      </div>
  


  
  <div class="filters">
    <button 
      v-for="category in ['All', 'Fresh', 'Floral', 'Woody']" 
      :key="category"
      @click="filterByCategory(category)"
    >
      {{ category }}
    </button>
  </div>
  
  <!-- Product display section -->
  <div class="featured">
    <h2>Best Sellers</h2>
    <div class="product-grid">
      <div 
        v-for="perfume in perfumes" 
        :key="perfume.id" 
        class="product-card"
      >
        <img :src="perfume.image" :alt="perfume.name">
        <h3>{{ perfume.name }}</h3>
        <p>{{ perfume.description }}</p>
        <p class="price">${{ perfume.price }}</p>
        <!-- Add stock indicator -->
        <p class="stock" :class="{ 'low-stock': perfume.stock < 5 }">
          In Stock: {{ perfume.stock }}
        </p>
        <button 
          @click="addToCart(perfume)"
          :disabled="perfume.stock === 0"
        >
          Add to Cart
        </button>
      </div>
    </div>
  </div>
</div>
</template>



<script setup lang="ts">
// This imports the tools we need from Vue
import { ref } from 'vue'
import 'primeicons/primeicons.css'

// Add interfaces for type safety
interface Perfume {
  id: number
  name: string
  description: string
  price: number
  image: string
  stock?: number  // Optional stock tracking
  category?: string  // Optional categorization
}

// Add shopping cart functionality
const cartItems = ref<Perfume[]>([])
const isCartVisible = ref(false)

const toggleCart = () => {
  isCartVisible.value = !isCartVisible.value
}

// Enhanced addToCart function
const addToCart = (perfume: Perfume) => {
  cartItems.value.push(perfume)
  // Show a more sophisticated notification
  // You might want to use a toast/notification library here
  alert(`Added ${perfume.name} to cart! Cart total: $${calculateTotal()}`)
}

// Calculate cart total
const calculateTotal = () => {
  return cartItems.value.reduce((total, item) => total + item.price, 0).toFixed(2)
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
])

// Filter perfumes by category
const filterByCategory = (category: string) => {
  if (category === 'All') {
    // Show all perfumes
    perfumes.value = ref<Perfume[]>([...perfumes.value])
  } else {
    // Filter perfumes based on the selected category
    perfumes.value = ref<Perfume[]>(perfumes.value.filter(perfume => perfume.category === category))
  }
}
</script>

<style>
/* Main container styles */
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
  padding: 1rem;
  /* 👇 CHANGE COLOR of the border under the nav */
  border-bottom: 1px solid #eee;
}

/* Navigation link styles */
.nav-links a {
  margin-left: 2rem;
  text-decoration: none;
  /* 👇 CHANGE COLOR of the menu text */
  color: #333;
}

/* Hero section styles */
.hero {
  text-align: center;
  padding: 4rem 2rem;
  /* 👇 CHANGE COLOR of the hero background */
  background: #f9f9f9;
  margin: 2rem 0;
}


/*shopping cart display*/
.shopping-cart {
  position: fixed;
  top: 0;
  right: 0;
  width: 300px;
  height: 100vh;
  background: white;
  box-shadow: -2px 0 5px rgba(0,0,0,0.2);
  padding: 1rem;
  z-index: 1000;  
}

.cart-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
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
  color: #665;
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
  .product-card {
    /* 👇 CHANGE COLORS of the card borders and background */
    border: 1px solid #eee;
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
  .price {
    font-weight: bold;
    font-size: 1.2rem;
    /* 👇 CHANGE COLOR of the price text */
    color: #2c3e50;
  }
  
  /* Button styles */
  button {
    /* 👇 CHANGE COLORS of the buttons */
    background: #000;  /* Button background color */
    color: white;      /* Button text color */
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    cursor: pointer;
  }
  
  /* Button hover effect */
  button:hover {
    /* 👇 CHANGE COLOR of the button when hovered */
    background: #333;
  }
  
  /* Add new styles */
  .cart-icon {
    position: relative;
    padding: 1rem;
    cursor: pointer;
  }
  
  .cart-count {
    position: absolute;
    top: 0;
    right: 0;
    background: #ff4444;
    color: white;
    border-radius: 50%;
    padding: 0.2rem 0.5rem;
    font-size: 0.8rem;
  }
  
  .filters {
    display: flex;
    gap: 1rem;
    padding: 1rem;
    justify-content: center;
  }
  
  .filters button {
    padding: 0.5rem 1rem;
    border: 1px solid #ddd;
    border-radius: 4px;
    background: white;
    cursor: pointer;
  }
  
  .filters button:hover {
    background: #f5f5f5;
  }
  
  .stock {
    color: #2c3e50;
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
      display: none; /* Consider adding a mobile menu */
    }
  }
</style>