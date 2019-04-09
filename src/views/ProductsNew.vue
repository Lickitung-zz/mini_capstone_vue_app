<template>

  <div class="root">
    <div v-for="error in errors">
      {{ error }}
    </div>
    <form v-on:submit.prevent="createProduct()">
      <p>Name <input type=text v-model="newProductName"></p>
      <p>Price <input type=text v-model="newProductPrice"></p>
      <p>Description <input type=text v-model="newProductDescription"></p>
      <p>Image Url <input type=text v-model="newProductImage"></p>
      <button v-on:click="createProduct()">Create Product</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      products: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImage: "",
      currentProduct: {},
      errors: []
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
    });
  },
  methods: {
    createProduct: function() {
      var params = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription,
        image_url: this.newProductImage
      };
      console.log('creating product');
      axios.post("/api/products", params).then(response => {
        console.log(response);
        this.$router.push("/");
      }).catch(error => {
        console.log("this isn't working.");
        console.log(error.response.data.errors);
        this.error = error.response.data.errors;
      });
    },
    updateProduct: function(theProduct) {
      console.log(theProduct);
      console.log('updating the recipe...');
      var params = {
        name: theProduct.name,
        price: theProduct.price,
        description: theProduct.description,
        image_url: theProduct.image_url
      };

      axios.patch("/api/products/" + theProduct.id, params).then(response => {
        console.log(response);
        theProduct = response.data;
      });
    },
  }
};
</script>
