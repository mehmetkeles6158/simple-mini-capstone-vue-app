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

    <button v-on:click="createProduct">Make Product</button>
    <hr />
    <div v-for="product in products" :key="product.id">
      <p>
        <b>Name:</b>
        {{ product.name }}
      </p>
      <p>
        <b>Price:</b>
        {{ product.price }}
      </p>
      <img v-bind:src="product.image_url" v-bind:alt="product.name" />
      <hr />
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
    createProduct: function () {
      console.log("create product");
      console.log(this.newProduct);
      axios
        .post("http://localhost:3000/products", {
          name: this.newProduct.name,
          price: this.newProduct.price,
          description: this.newProduct.description,
          url: this.newProduct.image_url,
        })
        .then((response) => {
          console.log(response.data);
        });
    },
  },
};
</script>
