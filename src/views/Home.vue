<template>
  <div class="home">
    <h1>{{ miniCapstone }}</h1>
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

    <button v-on:click="addProduct()">Add a product</button>
    <div v-bind:key="product.id" v-for="product in products">
      <p>
        {{ product.id }}. {{ product.name }}
        <button v-on:click="showProduct(product)">Show more info</button>
      </p>

      <div v-if="currentProduct == product">
        <p>Description: {{ product.description }}</p>
        <p>Price: {{ product.price }}</p>
        <img v-bind:src="product.image_url" />
        <div>
          <button v-on:click="openUpdate()">Update</button>

          <button v-on:click="destroyProduct(product)">Delete</button>
        </div>
        <div v-if="updatingProduct">
          <p>
            Name:
            <input type="text" v-model="product.name" />
          </p>
          <p>
            Description:
            <input type="text" v-model="product.description" />
          </p>
          <p>
            Price:
            <input type="text" v-model="product.price" />
          </p>
          <p>
            Image url:
            <input type="text" v-model="product.image_url" />
          </p>

          <button v-on:click="updateProduct(product)">Update the product</button>
        </div>
      </div>
      <hr />
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
      currentProduct: "",
      updatingProduct: false,
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
    showProduct: function(theProduct) {
      console.log("Showing info...");
      console.log(theProduct);
      this.currentProduct = theProduct;
    },
    updateProduct: function(theProduct) {
      console.log("Updating product info...");
      let params = {
        name: theProduct.name,
        price: theProduct.price,
        description: theProduct.description,
        image_url: theProduct.image_url,
      };
      axios.patch(`/api/products/${theProduct.id}`, params).then(response => {
        console.log(response.data);
        theProduct = response.data;
      });
    },
    openUpdate: function() {
      this.updatingProduct = true;
    },
    destroyProduct: function(theProduct) {
      console.log(theProduct);
      console.log("Deleting product...");
      alert("Are you sure you want to delete this product?");
      axios.delete(`/api/products/${theProduct.id}`).then(response => {
        console.log(response.data);

        let index = this.products.indexOf(theProduct.id);
        this.products.splice(index, 1);
      });
    },
  },
};
</script>

<style scoped>
body {
  background-color: #f1f3f4;
}
img {
  max-width: 400px;
  max-height: 400px;
}
/* 
br {
  width: 30%;
  height: 30%;
  align-content: center;
} */
</style>