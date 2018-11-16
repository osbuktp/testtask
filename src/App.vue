<template>
  <div id="app">
    <Product :product="this.products[0]"></Product>
  </div>
</template>

<script>
import Product from "./components/Product.vue"
import ProductsJSON from './server/products.json'

export default {
  name: "app",
  data() {
    return {
      products: ProductsJSON
    }
  },
  components: {
    Product
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
