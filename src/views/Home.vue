<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h3>{{ miniCapstone }}</h3>
    <p>
      Name:
      <input type="text" v-model="newProductName" />
    </p>
    <p>
      Price:
      <input type="text" v-model="newProductPrice" />
    </p>
    <p>
      Description:
      <input type="text" v-model="newProductDescription" />
    </p>
    <p>
      Image url:
      <input type="text" v-model="newProductImageUrl" />
    </p>

    <button v-on:click="addProduct()">Add a Product</button>
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
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImageUrl: "",
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
    });
  },
  methods: {
    addProduct: function() {
      let params = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription,
        image_url: this.newProductImageUrl,
      };

      axios.post("/api/products", params).then(response => {
        console.log("Creating product");
        this.products.push(response.data);
        this.newProductName = "";
        this.newProductPrice = "";
        this.newProductDescription = "";
        this.newProductImageUrl = "";
      });
    },
  },
};
</script>
