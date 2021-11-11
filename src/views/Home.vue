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
      <p><button v-on:click="showProduct(product)">More Info</button></p>
      <hr />
    </div>
    <dialog id="show-product">
      <form method="dialog">
        <p>
          <b>Name:</b>
          {{ currentProduct.name }}
        </p>
        <p>
          <b>Price:</b>
          {{ currentProduct.price }}
        </p>
        <p>
          <b>Description:</b>
          {{ currentProduct.description }}
        </p>
        <p>
          <b>Image Url:</b>
          {{ currentProduct.image_url }}
        </p>
        <button>Close</button>
      </form>
    </dialog>
    <hr />
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
      currentProduct: {},
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
    showProduct: function (theProduct) {
      console.log(theProduct);
      this.currentProduct = theProduct;
      console.log("opening modal...");
      document.querySelector("#show-product").showModal();
    },
  },
};
</script>
