<template>
  <div class="root">
    <h1>{{ message }}</h1>
    <!-- <h1>name: {{ products }}</h1> -->
    <input type="text" v-model="nameFilter">
    <div v-for="product in filterBy(products, nameFilter, 'name')"">
      <p><div id="product">Name: </div>{{ product.name }}</p>
      <!-- <p><div id="product">Price: </div>${{ product.price }}</p>
      <p><div id="product">Description: </div>{{ product.description }}</p> -->
     <!--  <p><img v-bind:src="product.image_url"></p> -->
      <router-link v-bind:to="'/products/' + product.id">See more info</router-link>
      <hr>
    </div>
  </div>
</template>

<style>
/*  .home {
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
  }*/
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      currentProduct: {},
      nameFilter: ""
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
      });
    },
    toggleProduct: function(theProduct) {
      if (this.currentProduct === theProduct) {
        this.currentProduct = {};
      } else {
        this.currentProduct = theProduct;
      }
      console.log('toggling info...');
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
    deleteProduct: function(theProduct) {
      console.log("deleting product...");
      axios.delete("/api/products/" + theProduct.id).then(response => {
        console.log(response);
        var index = this.products.indexOf(theProduct);
        this.products.splice(index, 1);
      });
    }
  }
};
</script>
