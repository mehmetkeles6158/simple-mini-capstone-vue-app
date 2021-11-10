<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>
      name:
      <input type="text" v-model="newProduct.name" />
    </p>
    <p>
      price:
      <input type="text" v-model="newProduct.price" />
    </p>
    <p>
      description:
      <input type="text" v-model="newProduct.description" />
    </p>
    <p>
      image_url:
      <input type="text" v-model="newProduct.image_url" />
    </p>

    <button v-on:click="createProducts">Make Product</button>
    <div v-for="product in products" :key="product.id">
      <p>{{ product.name }}</p>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Hello Vue.js! This is my first framework' project",
      products: [],
      newProduct: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      console.log("getting products from api");
      axios.get("http://localhost:3000/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    createProducts: function () {
      console.log("create product");
      axios
        .post("http://localhost:3000/products", {
          input_name: this.newRecipe.name,
          input_price: this.newRecipe.price,
          input_description: this.newRecipe.input_description,
          image_url: this.newRecipe.image_url,
        })
        .then((response) => {
          console.log(response.data);
        });
    },
  },
};
</script>
