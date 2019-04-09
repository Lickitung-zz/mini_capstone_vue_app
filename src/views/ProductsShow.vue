<template>
  <div class="root">
    <p>Name: {{ product.name }}</p>
    <p>Description: {{ product.description }}</p>
    <p>Price: {{ product.price }}</p>
    <p><img v-bind:src="product.image_url"></p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      product: {
        name: "cup",
        description: "a cup",
        price: "20",
        image_url: "n/a"
      }
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id).then(response => {
      console.log(response.data);
      this.product = response.data;
    });
  },
  methods: {}
};
</script>