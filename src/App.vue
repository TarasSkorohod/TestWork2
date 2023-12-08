<!-- src/App.vue -->
<template>
  <div id="app">
    <ProductList />
  </div>
</template>

<script>
import ProductList from './components/ProductList.vue';

export default {
  name: 'App',
  components: {
    ProductList,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
