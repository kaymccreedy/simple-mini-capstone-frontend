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
      currentProduct: {},
      editProductParams: {},
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
    showProduct: function (product) {
      console.log(product);
      this.currentProduct = product;
      this.editProductParams = product;
      document.querySelector("#product-info").showModal();
    },
    updateProduct: function (product) {
      axios.patch("http://localhost:3000/products/" + product.id, this.editProductParams).then((response) => {
        console.log("Success!", response.data);
      });
    },
    destroyProduct: function (product) {
      axios.delete("http://localhost:3000/products/" + product.id).then((response) => {
        console.log("Success", response.data);
      });
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
    <br />
    <p v-if="showErrorMessage">Error: {{ errorMessage }}</p>
    <div>
      <h3>Products</h3>
    </div>
    <div v-for="product in products" v-bind:key="product.id">
      <h5>{{ product.name }}</h5>
      <img :src="product.image_url" :alt="product.description" width="500" />
      <br />
      <button v-on:click="showProduct(product)">Show Info</button>
      <br />
      <br />
    </div>
    <dialog id="product-info">
      <form method="dialog">
        <h5>Edit Recipe</h5>
        <h6>
          Name:
          <input type="text" v-model="editProductParams.name" />
        </h6>
        <h6>
          Description:
          <input type="text" v-model="editProductParams.description" />
        </h6>
        <h6>
          Price: $
          <input type="text" v-model="editProductParams.price" />
        </h6>
        <h6>
          Image URL:
          <input type="text" v-model="editProductParams.image_url" />
        </h6>
        <h6>Tax: ${{ currentProduct.tax }}</h6>
        <h6>Total: ${{ currentProduct.total }}</h6>
        <button v-on:click="updateProduct(currentProduct)">Update</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>
