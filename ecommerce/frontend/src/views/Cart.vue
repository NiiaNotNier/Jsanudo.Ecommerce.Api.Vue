<template>
  <div class="cart">
    <div>
      <h1>Cart</h1>
      <hr>
      <template>
        <div>
          <section class="list">
            <ul>
              <li v-for="product in products" :key="product.id">
                <p>Name: {{ product.productName }}</p>
                <p>Price: {{ product.productPrice }} €</p>
                <p>Quantity: {{ product.quantity }}</p>
                <b-button @click="moreProducts(product.id,product.quantity,product.productId,product.productName,product.productPrice)">
                  More
                </b-button>
                <b-button @click="lessProducts(product.id,product.productId,product.productName,product.productPrice)">
                  Less
                </b-button>
                <b-button @click="remove(product.id,product.quantity,product.productId,product.productName,product.productPrice)">
                  Remove
                </b-button>
                <div>
                  <p>Pack price: {{packPrice(product.quantity, product.productPrice)}} €</p>
                </div>
                <hr>
              </li>
            </ul>
          </section>
        </div>
        Total: {{totalCart}} $
        <router-link :to="{ name: 'Orders' }">
          <b-button @click="buy()">
            Buy products
          </b-button>
        </router-link>
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
    moreProducts(id, quantity, productId, productName, productPrice) {
      fetch(api_url("/cart/" + id), {
        method: "PUT",
        body: JSON.stringify({
          productId: productId,
          productName: productName,
          quantity: quantity + 1,
          productPrice: productPrice,
        }),
        headers: {
          "Content-Type": "application/json",
        },
      });
      fetch(api_url("/cart/"))
        .then((result) => result.json())
        .then((data) => (this.products = data));
    },
    lessProducts(id, quantity, productId, productName, productPrice) {
      if (quantity > 1) {
        fetch(api_url("/cart/" + id), {
          method: "PUT",
          body: JSON.stringify({
            productId: productId,
            productName: productName,
            quantity: quantity - 1,
            productPrice: productPrice,
          }),
          headers: {
            "Content-Type": "application/json",
          },
        });
        fetch(api_url("/cart/"))
          .then((result) => result.json())
          .then((data) => (this.products = data));
      } else {
        fetch(api_url("/cart/" + id), {
          method: "DELETE",
          body: JSON.stringify({
            productId: productId,
            productName: productName,
            quantity: quantity,
            productPrice: productPrice,
          }),
          headers: {
            "Content-Type": "application/json",
          },
        });
        fetch(api_url("/cart/"))
          .then((result) => result.json())
          .then((data) => (this.products = data));
      }
    },
    remove(id, quantity, productId, productName, productPrice) {
      fetch(api_url("/cart/" + id), {
        method: "DELETE",
        body: JSON.stringify({
          productId: productId,
          productName: productName,
          quantity: quantity,
          productPrice: productPrice,
        }),
        headers: {
          "Content-Type": "application/json",
        },
      });
      fetch(api_url("/cart/"))
        .then((result) => result.json())
        .then((data) => (this.products = data));
    },
    packPrice(productPrice, quantity) {
      return parseFloat(productPrice) * parseFloat(quantity);
    },
    buy() {
      for (let i = 0; i < this.products.length; i++) {
        fetch(api_url("/orders/"), {
          method: "POST",
          body: JSON.stringify({
            productId: this.products[i].id,
            productName: this.products[i].productName,
            quantity: this.products[i].quantity,
            productPrice: this.products[i].productPrice,
            packPrice:
              this.products[i].quantity * this.products[i].productPrice,
          }),
          headers: {
            "Content-Type": "application/json",
          },
        });
      }
      fetch(api_url("/orders/"), {
        method: "POST",
        body: JSON.stringify({
          totalOrder: this.totalCart,
        }),
        headers: {
          "Content-Type": "application/json",
        },
      });
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
  },
  computed: {
    totalCart: function () {
      let sum = 0;
      for (let i = 0; i < this.products.length; i++) {
        sum +=
          parseFloat(this.products[i].productPrice) *
          parseFloat(this.products[i].quantity);
      }

      return sum;
    },
  },
};
</script>