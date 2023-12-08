<template>
  <div class="product-list-container">
    <div class="filter-section">
      <label for="category">Category:</label>
      <select v-model="selectedCategory" @change="filterProducts">
        <option value="">All</option>
        <option v-for="category in categories" :key="category" :value="category">
          {{ category }}
        </option>
      </select>
    </div>
    <div class="filter-section">
      <label for="sort">Sort By:</label>
      <select v-model="sortBy" @change="sortProducts">
        <option value="price">Price</option>
        <option value="rating">Rating</option>
      </select>
    </div>
    <div class="product-list" ref="productList" @scroll="handleScroll">
      <product-card class="product-card" v-for="product in filteredProducts" :key="product.id" :product="product" />
    </div>

    <button class="load-more-btn" @click="loadMore">Load More</button>
  </div>
</template>


<script>
import axios from 'axios';
import ProductCard from './ProductCard.vue';

export default {
  components: {
    ProductCard,
  },
  data() {
    return {
      products: [],
      categories: [],
      selectedCategory: '',
      sortBy: 'price',
      page: 1,
      pageSize: 5,
      scrolling: false,
    };
  },
  computed: {
    filteredProducts() {
      let filtered = [...this.products];
      if (this.selectedCategory) {
        filtered = filtered.filter(product => product.category === this.selectedCategory);
      }
      return filtered.sort((a, b) => (this.sortBy === 'price' ? a.price - b.price : b.rating.rate - a.rating.rate));
    },
  },
  methods: {
    async fetchProducts() {
      try {
        const response = await axios.get('https://fakestoreapi.com/products');
        this.products = response.data;
        this.categories = [...new Set(this.products.map(product => product.category))];
      } catch (error) {
        console.error('Error fetching products:', error);
      }
    },

    filterProducts() {
      //testW
    },
    sortProducts() {
      //testW
    },
    handleScroll() {
      const container = this.$refs.productList;

      if (container.scrollTop + container.clientHeight >= container.scrollHeight - 50 && !this.scrolling) {
        this.loadMore();
      }
    },
    async loadMore() {
      this.page++;
      this.scrolling = true;

      try {
        const response = await axios.get(`https://fakestoreapi.com/products?limit=${this.page * this.pageSize}`);
        const newProducts = response.data;
        this.products = [...this.products, ...newProducts];
      } catch (error) {
        console.error('Error fetching more products:', error);
      } finally {
        this.scrolling = false;
      }
    },
  },
  mounted() {
    this.fetchProducts();
  },
};
</script>
<style scoped>
.product-list-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.filter-section {
  margin-bottom: 20px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.label {
  margin-right: 10px;
  font-weight: bold;
}

select, button {
  padding: 8px;
  font-size: 14px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-bottom: 10px;
}

.load-more-btn {
  margin: 10px;
  background-color: #4c89af;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.load-more-btn:hover {

  background-color: #4565a0;
}

.product-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  align-items: stretch;
}

.product-card {
  width: calc(33.33% - 20px);
  box-sizing: border-box;
}

@media (max-width: 768px) {
  .product-card {
    width: calc(50% - 20px);
  }
}

@media (max-width: 480px) {
  .product-card {
    width: 100%;
  }
}
</style>
