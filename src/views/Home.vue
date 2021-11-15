<template>
  <div class="home">
    <h1>{{ errors }}</h1>
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
        <b>Id:</b>
        {{ product.id }}
      </p>
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
      <p><button v-on:click="deleteProduct(product)">Delete product</button></p>
      <hr />
    </div>
    <dialog id="show-product">
      <form method="dialog">
        <p>
          <b>Name:</b>
          <input type="text" v-model="currentProduct.name" />
        </p>
        <p>
          <b>Price:</b>
          <input type="text" v-model="currentProduct.price" />
        </p>
        <p>
          <b>Description:</b>
          <input type="text" v-model="currentProduct.description" />
        </p>
        <p>
          <b>Image Url:</b>
          <input type="text" v-model="currentProduct.image_url" />
        </p>
        <button v-on:click="updateProduct(currentProduct)">Edit Product</button>
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
      errors: [],
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
          this.newProduct = {};
        })
        .catch((error) => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    },
    showProduct: function (theProduct) {
      console.log(theProduct);
      this.currentProduct = theProduct;
      console.log("opening modal...");
      document.querySelector("#show-product").showModal();
    },
    updateProduct: function (theProduct) {
      console.log("updating product!");
      var productParams = {
        name: theProduct.name,
        price: theProduct.price,
        description: theProduct.description,
        url: theProduct.image_url,
      };
      axios.patch(`http://localhost:3000/products/${theProduct.id}`, productParams).then((response) => {
        console.log(response.data);
      });
    },
    deleteProduct: function (theProduct) {
      console.log(theProduct.id);
      console.log("deleting product !");
      axios.delete(`http://localhost:3000/products/${theProduct.id}`).then((response) => {
        console.log(response.data);
        var index = this.products.indexOf(theProduct);
        this.products.splice(index, 1);
      });
    },
  },
};
</script>
