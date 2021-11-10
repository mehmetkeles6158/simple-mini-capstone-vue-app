<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="product in products" :key="product.id">
      <p>{{ product.name }}</p>
    </div>
  </div>
</template>

<style></style>

<script>
export default {
  data: function () {
    return {
      message: "Hello Vue.js! This is my first framework' project",
      products: [],
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      console.log("getting products from api");
      // eslint-disable-next-line no-undef
      axios.get("http://localhost:3000/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
  },
};
</script>
