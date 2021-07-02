<template>
  <div class="home">
    <!-- <h1>New Product!</h1>
    <button v-on:click="createProduct">Create!</button> -->
    <div>
      <h1>Add A Product!</h1>
      <!-- <input type="text" v-model="name" placeholder="name" />
      <input type="text" v-model="price" placeholder="price" />
      <input type="text" v-model="description" placeholder="description" />
      <input type="text" v-model="image_url" placeholder="image_url" /> -->
      <input type="text" v-model="newProductName" placeholder="name" />
      <input type="text" v-model="newProductPrice" placeholder="price" />
      <input type="text" v-model="newProductDescription" placeholder="description" />
      <input type="text" v-model="newProductImage_Url" placeholder="image_url" />
      <button v-on:click="createProducts">Create New Product</button>
    </div>

    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Info!</h1>
        <p>Name: {{ currentProduct.name }}</p>
        <p>Price: {{ currentProduct.price }}</p>
        <p>Description: {{ currentProduct.description }}</p>
        <button>See Ya</button>
      </form>
    </dialog>

    <br />
    <h1>All Our Products!</h1>
    <div>
      <div v-for="product in products" v-bind:key="product.id">
        <p>
          <b>{{ product.name }} - ${{ product.price }}</b>
          <button v-on:click="showProduct(product)">More Info</button>
        </p>
        <img v-bind:src="product.image_url" v-bind:alt="product.title" />
      </div>
    </div>
  </div>
</template>
<style>
img {
  max-width: 250px;
}
</style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      products: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImage_Url: "",
      currentProduct: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("/api/products").then((response) => {
        this.products = response.data;
        console.log("all products: ", this.products);
      });
    },
    createProducts: function () {
      var params = {
        name: this.newProductName,
        description: this.newProductDescription,
        price: this.newProductPrice,
        image_url: this.newProductImage_Url,
        // name: "example",
        // description: "example",
        // price: "example",
        // image_url: "example",
      };
      axios
        .post("/api/products", params)
        .then((response) => {
          console.log("success: ", response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showProduct: function (product) {
      this.currentProduct = product;
      console.log(product);
      document.querySelector("#product-details").showModal();
    },
  },
};
</script>
