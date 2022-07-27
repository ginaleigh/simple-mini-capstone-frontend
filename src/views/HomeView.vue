<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to our specialty shop!",
      products: [],
      newProductName: "",
      newDescription: "",
      newImage_url: "",
      newPrice: "",
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products.json").then((response) => {
        this.products = response.data;
        console.log("All products: ", response.data);
      });
    },
    createProduct: function () {
      console.log("create product");

      var params = {
        name: this.newProductName,
        description: this.newDescription,
        image_url: this.newImage_url,
        price: this.newPrice,
      };

      axios
        .post("http://localhost:3000/products.json", params)
        .then((response) => {
          console.log("Cool!", response.data);
          this.products.push(response.data);
          this.newProductName = "";
          this.newDescription = "";
          this.newImage_url = "";
          this.newPrice = "";
        })
        .catch((error) => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>New Product</h1>
    Name:
    <input type="text" v-model="newProductName" />
    Description:
    <input type="text" v-model="newDescription" />
    Image_url:
    <input type="text" v-model="newImage_url" />
    Price:
    <input type="text" v-model="newPrice" />
    <button v-on:click="createProduct()">Create Product</button>
    <div class="errors" v-for="error in errors" v-bind:key="error">
      {{ error }}
    </div>
    <h1>{{ message }}</h1>
    <div v-for="product in products" v-bind:key="product.id">
      <h2>Name: {{ product.name }}</h2>
      <img v-bind:src="product.image_url" v-bind:alt="product.title" />
      <p>Price: {{ product.price }}</p>
    </div>
  </div>
</template>

<style></style>
