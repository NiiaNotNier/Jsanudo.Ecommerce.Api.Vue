<template>
  <div class="cart">
    <div>
      <template>
        <h1>Carrito</h1>
        <div>
          <div v-for="product in products" :key="product.id" style="margin-left: 40%; margin-top: 10px; display: flex;">
            {{ product.productName }}
            {{ product.productPrice }} €
            <b-button style="margin-left: 20px;"
                  @click="remove(product.id)">Quitar
            </b-button>
          </div>
          <router-link :to="{ name: 'Orders' }">
          <b-button
            @click="buy()">Comprar
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
  name: "Cart",

  components: {},
  created() {
    fetch(api_url("/cart/"))
      .then((result) => result.json())
      .then((data) => (this.products = data));
  },
  data() {
    return {
      products: [],
    };
  },
  methods: {
    remove(id, productId) {
      fetch(api_url("/cart/" + id), {
        method: "DELETE",
        body: JSON.stringify({
          productId: productId}),
        headers: {
          "Content-Type": "application/json",
        },
      });
      fetch(api_url("/cart/"))
        .then((result) => result.json())
        .then((data) => (this.products = data));
    },
    buy() {
      for (let i = 0; i < this.products.length; i++) {
        fetch(api_url("/orders/"), {
          method: "POST",
          body: JSON.stringify({
            productId: this.products[i].id,
            productName: this.products[i].productName,
            productPrice: this.products[i].productPrice
          }),
          headers: {
            "Content-Type": "application/json",
          },
        });
      }
      for(let i = 0; i <= this.products.length+1; i++){
        fetch(api_url("/cart/" + i), {
          method: "DELETE",
          body: JSON.stringify({}),
          headers: {
            "Content-Type": "application/json",
          },
        });
      }
    },
  }
};
</script>