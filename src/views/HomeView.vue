<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Simple Mini Capstone",
      subtitle: "This is the Front End",
      products: [],
      newProductParams: {},
      showErrorMessage: false,
      errorMessage: "",
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products").then((response) => {
        this.products = response.data;
        console.log("All Products: ", this.products);
      });
    },
    createProduct: function () {
      axios
        .post("http://localhost:3000/products/", this.newProductParams)
        .then((response) => {
          console.log("Success!", response.data);
          this.products.push(response.data);
          this.newProductParams = {};
          this.showErrorMessage = false;
        })
        .catch((error) => (this.errorMessage = error))
        .then((this.showErrorMessage = true));
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>{{ subtitle }}</h2>
    <h3>New Product</h3>
    <div>
      Name:
      <input type="text" v-model="newProductParams.name" />
      <br />
      Description:
      <input type="text" v-model="newProductParams.description" />
      <br />
      Image URL:
      <input type="text" v-model="newProductParams.image_url" />
      <br />
      Price:
      <input type="text" v-model="newProductParams.price" />
      <br />
    </div>
    <button v-on:click="this.createProduct()">Create</button>
    <br />
    <p v-if="showErrorMessage">Error: {{ errorMessage }}</p>
    <div v-for="product in products" v-bind:key="product.id">
      <h5>{{ product.name }}</h5>
      <h6>${{ product.price }}</h6>
      <img :src="product.image_url" :alt="product.description" width="500" />
    </div>
  </div>
</template>

<style></style>
