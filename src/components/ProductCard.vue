<template>
  <div class="product-card">
    <div class="product-image">
      <img :src="product.image" alt="Product Image" />
    </div>
    <div class="product-details">
      <h3>{{ product.title }}</h3>
      <p class="price">{{ product.price }} $</p>
      <div class="rating">
        <span v-for="n in Math.round(product.rating.rate)" :key="n" class="star">★</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    product: Object,
  },
};
</script>

<style scoped>
.product-card {
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease-in-out;
  cursor: pointer;
  background-color: #fff;
  display: flex;
}

.product-card:hover {
  transform: scale(1.05);
}

.product-image {
  position: relative;
  overflow: hidden;
}

.product-card img {
  width: 100%;
  margin: 0 auto;
  object-fit: cover;
}

.product-details {
  padding: 15px;
  flex-grow: 1;
}

.product-details h3 {
  margin: 0 0 10px;
  font-size: 18px;
}

.price {
  font-weight: bold;
  font-size: 16px;
  color: #333;
}

.rating {
  display: flex;
  align-items: center;
}

.star {
  color: #ff9900;
  font-size: 18px;
  margin-right: 4px;
}
</style>
