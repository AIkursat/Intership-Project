<template>
<!-- <img alt="Vue logo" src="./assets/img/logo.png"> -->
<img src="./assets/img/AxaTr.jpg" alt="AXA">
  <PostProduct @init="init" />
  <Products :products="products" />
  
  
</template>

<script>
import Products from "./components/Products.vue"
import PostProduct from "./components/PostProduct.vue"
import axios from "axios"
import "./assets/style/style.css"
export default {
  name: 'App',
  components: {
    Products,
    PostProduct
  },
  data: function(){
    return{
      products: []
    }
  },
   created: function () {
    this.init()
  },
   methods: {
    init(){
    axios.get("https://localhost:44375/api/products").then((res) => {
      this.products = res.data;
      console.log(this.products[0])
    });}
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
