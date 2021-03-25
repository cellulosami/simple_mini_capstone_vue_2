/* eslint-disable prettier/prettier */

<template>
  <div class="home">
    <h1 >{{ message }}</h1>
    <p>Name -- <input v-model="params.name"></p>
    <p>Price -- <input v-model="params.price"></p>
    <p>Description -- <input v-model="params.description"></p>
    <p>Image URL -- <input v-model="params.image_url"></p>
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
      <dialog id="product-details" class="modal-content" style="text-align:left;">
        <form method="dialog">
          <span v-on:click="closeModal" class="close" style="width: 24px; height: 24px;">&times;</span>v-on:click="closeModal" 
          <p><b>Name:</b> <input v-model="currentProduct.name"></p>
          <p><b>Description:</b> <input v-model="currentProduct.description"></p>
          <p><b>Price:</b> <input v-model="currentProduct.price"></p>
          <p><b>Image URL:</b> <input v-model="currentProduct.image_url"></p>
          <button v-on:click="productsUpdate">Update</button>
          <button v-on:click="productsDestroy">Delete</button>
        </form>
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
  padding-top: 0px;
  padding-bottom: 0px;
  padding-right: 0px;
  padding-left: 0px;
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
      currentProduct: {},
      showModal: false
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
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
    },
    closeModal: function () {
      document.querySelector("#product-details").close();
    },
    productsUpdate: function () {
      let parameters = this.currentProduct;
      axios
        .patch("http://localhost:3000/api/products/" + this.currentProduct.id, parameters)
        .then(response => {
          console.log(response.data);
        })
    },
    productsDestroy: function() {
      console.log(this.currentProduct);
      axios
        .delete("http://localhost:3000/api/products/" + this.currentProduct.id)
        .then(response => {
          console.log(response.data);
          let index = this.products.indexOf(this.currentProduct);
            this.products.splice(index, 1);
        })
    }
  },
};
</script>
