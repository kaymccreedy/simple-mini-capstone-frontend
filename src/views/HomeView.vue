<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Simple Mini Capstone",
      subtitle: "It's a Mock-Up Web Store!",
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
      axios.get("products").then((response) => {
        this.products = response.data;
        console.log("All Products: ", this.products);
      });
    },
    newProduct: function () {
      document.querySelector("#product-create").showModal();
    },
    createProduct: function () {
      axios
        .post("products/", this.newProductParams)
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
      axios.patch("products/" + product.id, this.editProductParams).then((response) => {
        console.log("Success!", response.data);
      });
    },
    destroyProduct: function (product) {
      axios.delete("products/" + product.id).then((response) => {
        console.log("Success", response.data);
        var index = this.products.indexOf(product);
        this.products.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>{{ subtitle }}</h2>
    <br />
    <button v-on:click="newProduct()">Add a Product</button>
    <div>
      <dialog id="product-create">
        <form method="dialog">
          <h5>Add a Product</h5>
          <h6>
            Name:
            <input type="text" v-model="newProductParams.name" />
          </h6>
          <h6>
            Description:
            <input type="text" v-model="newProductParams.description" />
          </h6>
          <h6>
            Price: $
            <input type="text" v-model="newProductParams.price" />
          </h6>
          <h6>
            Image URL:
            <input type="text" v-model="newProductParams.image_url" />
          </h6>
          <button v-on:click="this.createProduct()">Create</button>
          <button>Close</button>
        </form>
      </dialog>
    </div>
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
        <h5>Edit Product</h5>
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
        <button v-on:click="destroyProduct(currentProduct)">Delete</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>
