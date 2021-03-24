/* eslint-disable prettier/prettier */

<template>
  <div class="home">
    <h1 >{{ message }}</h1>
    <p><input v-model="params.name"> -- Name</p>
    <p><input v-model="params.description"> -- Description</p>
    <p><input v-model="params.price"> -- Price</p>
    <p><input v-model="params.image_url"> -- Image URL</p>
    <div></div>
    <button v-on:click="createProducts">Create new product</button>
    <hr>
    <div style="font-weight: bold; font-size: 20px;" v-for="product in products" v-bind:key="product.id">
      {{ product.name }}
      <p><button v-on:click="showProducts(product)">More info</button></p>
      <div><img v-bind:src="product.image_url"></div>
      <hr>
    </div>
    <div class="modal">
      <dialog class="modal-content" open style="text-align:left;">
        <span class="close">&times;</span>
        <p><b>Name:</b> {{ currentRecipe.name }}</p>
        <p><b>Description:</b> {{ currentRecipe.description }}</p>
        <p><b>Price:</b> ${{ currentRecipe.price }}</p>
        <p><b>ID:</b> {{ currentRecipe.id }}</p>
      </dialog>
    </div>
  </div>
</template>

<style>
body {
  background-color: rgb(244, 244, 240);
}

img {
  width: 250px;
}

.modal {
  /* display: none; Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0, 0, 0); /* Fallback color */
  background-color: rgba(0, 0, 0, 0.4); /* Black w/ opacity */
}

.modal-content {
  color: rgb(60, 60, 60);
  background-color: #f5e8d5;
  margin: 15% auto; /* 15% from the top and centered */
  padding: 20px;
  border: 5px solid rgb(240, 221, 180);
  width: 80%; /* Could be more or less, depending on screen size */
}

.close {
  color: #aaa;
  float: right;
  font-size: 28px;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
</style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Woah nelly",
      products: [],
      params: { name: "", description: "", price: "", image_url: "" },
      currentRecipe: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios
        .get("http://localhost:3000/api/products")
        .then((response) => {
          this.products = response.data;
        });
    },
    createProducts: function () {
      console.log("CREATE CREATE CREATE");
      console.log(this.params);
      axios
        .post("http://localhost:3000/api/products", this.params)
        .then((response) => {
          this.products.push(response.data);
        });
    },
    showProducts: function (product) {
      console.log(product);
      this.currentRecipe = product;
    },
  },
};
</script>
