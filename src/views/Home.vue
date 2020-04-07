<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h3>{{ miniCapstone }}</h3>
    <div v-bind:key="products.id" v-for="product in products">
      <p>{{ product.id }}. {{ product.name }}</p>
      <img v-bind:src="product.image_url" />
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      miniCapstone: "Mini Captsone App",
      products: [],
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
    });
  },
  methods: {},
};
</script>
