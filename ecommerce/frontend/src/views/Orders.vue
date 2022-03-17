<template>
  <div class="checkout">
    <div>
      <template>
        <div>
            <p>Felicidades, lo has comprado</p>
            <router-link :to="{ name: 'Home' }">
            <b-button>
              Home
            </b-button>
            </router-link>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
import api_url from "../utils/api";

export default {
  name: "Home",

  components: {},
  created() {
    fetch(api_url("/products"))
      .then((result) => result.json())
      .then((data) => (this.products = data));
  },
  data() {
    return {
      products: [],
    };
  },
  methods: {
    addtoCart(productId) {
      fetch(api_url("/cart/"), {
        method: "POST",
        body: JSON.stringify({
          productId: productId
        }),
        headers: {
          "Content-Type": "application/json",
        },
      });
    },
  },
};
</script>
