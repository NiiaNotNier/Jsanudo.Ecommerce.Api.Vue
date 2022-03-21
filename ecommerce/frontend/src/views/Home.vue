<template>
  <div class="home">
    <div>
      <h1>Products</h1>
      <template>
        <div>
          <section class="list">
            <ul class="products">
              <li style="width: 310px; display: inline-grid; margin: 20px;"
                v-for="product in products" :key="product.id" class="product">
                <img class="image" :src="product.mainImage" alt width="100%" />
                <p class="name" style="margin-top: 10px;">
                 {{ product.name }}
                </p>
                <p>
                  <em>${{ product.price }}</em>
                </p>
                <div style="display: flex">
                  <router-link :to="{ name: 'Details', params: { id: product.id } }" style="width: 45%">
                    <b-button style="width: 90%;">Details</b-button>
                  </router-link>
                  <b-button @click="addCart(product.id, product.name, product.price)" style="width: 45%; margin-left: 5%;">Add to cart</b-button>
                </div>
              </li>
            </ul>
          </section>
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
    addCart(productId, productName, productPrice) {
      fetch(api_url("/cart/"), {
        method: "POST",
        body: JSON.stringify({
          productId: productId,
          productName: productName,
          quantity: 1,
          productPrice: productPrice,
        }),
        headers: {
          "Content-Type": "application/json",
        },
      });
    },
  },
};
</script>
