<template>
  <div class="root">
    <div v-for="error in errors">
      {{ error }}
    </div>
    <form v-on:submit.prevent="updateProduct()">
      <p>Name <input type=text v-model="product.name"></p>
      <p>Price <input type=text v-model="product.price"></p>
      <p>Description <input type=text v-model="product.description"></p>
      <p>Image Url <input type=text v-model="product.image_url"></p>
      <input type="submit" value="Update product">
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      product: {
        name: "",
        price: "",
        description: "",
        image: "",
        id: ""
      },
      errors: []
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id).then(response => {
      console.log(response.data)
      this.products = response.data;
    });
  },
  methods: {
    updateProduct: function() {
      console.log('updating the recipe...');
      var params = {
        name: this.product.name,
        price: this.product.price,
        description: this.product.description,
        image_url: this.product.image_url
      };
      console.log(params);

      axios.patch("/api/products/" + this.$route.params.id, params).then(response => {
        console.log(response);
        this.$router.push("/products/" + this.$route.params.id).catch(error => {
          console.log('not working');
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
      });
    },
  }
};
</script>
