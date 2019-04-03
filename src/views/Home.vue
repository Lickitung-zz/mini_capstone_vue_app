<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <!-- <h1>name: {{ products }}</h1> -->
    <p>Name <input type=text></input></p>
    <p>Price <input type=text></input></p>
    <p>Description <input type=text></input></p>
    <button class="createProduct">Create Product</button>
    <hr>
    <div v-for="product in products">
      <p><div id="product">Name: </div>{{ product.name }}</p>
      <p><div id="product">Price: </div>${{ product.price }}</p>
      <p><div id="product">Description: </div>{{ product.description }}</p>
      <p><img v-bind:src="product.image_url"></p>
      <div id="hr"><hr></div>
    </div>
  </div>
</template>

<style>
  .home {
    background: linear-gradient(124deg, #ff2400, #e81d1d, #e8b71d, #e3e81d, #1de840, #1ddde8, #2b1de8, #dd00f3, #dd00f3);
      background-size: 1800% 1800%;

      -webkit-animation: rainbow 2s ease infinite;
      -z-animation: rainbow 2s ease infinite;
      -o-animation: rainbow 2s ease infinite;
        animation: rainbow 2s ease infinite;}

      @-webkit-keyframes rainbow {
          0%{background-position:0% 82%}
          50%{background-position:100% 19%}
          100%{background-position:0% 82%}
      }
      @-moz-keyframes rainbow {
          0%{background-position:0% 82%}
          50%{background-position:100% 19%}
          100%{background-position:0% 82%}
      }
      @-o-keyframes rainbow {
          0%{background-position:0% 82%}
          50%{background-position:100% 19%}
          100%{background-position:0% 82%}
      }
      @keyframes rainbow { 
          0%{background-position:0% 82%}
          50%{background-position:100% 19%}
          100%{background-position:0% 82%}
}

  .home #hr {
    border-color: blue;
    color: blue;
  }

  .home #product {
    color: purple;
  }

  .home img {
    width: 700px;
  }
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      products: []
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
    });
  },
};
</script>
