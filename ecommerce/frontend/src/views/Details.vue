<template>
  <div class="details">
    <div>
      <h1>Details of {{ product.id }}</h1>

      <template>
        <div>
          <section class="wrapper">
            <ul class="products">
              <li style=" width: 70%; display: inline-grid; margin: 20px;" class="producto">
                <div class="box" style="display: flex;">
                    <div class="imageBox" style="width: 40%">
                        <img class="image" :src="product.mainImage" alt width="100%" />
                    </div>
                    <div class="data" style="width: 60%">
                        <br><br>{{ product.name }}
                        <br><br>{{ product.description }}
                        <br><br>${{ product.price }}
                    </div>
                </div>
                <div style="display: flex; margin-top: 40px;">
                  <router-link :to="{ name: 'Home' }" style="text-decoration: none; width: 45%">
                    <b-button>
                      Home
                    </b-button>
                  </router-link>
                  <b-button @click="addtoCart">
                    Add to cart
                  </b-button>
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
  name: "Details",
  components: {},
  created() {
    this.getProductById(this.$route.params.id);
  },
  beforeRouteUpdate(to) {
    this.getProductById(to.params.id);
  },
  data() {
    return {
      product: [],
    };
  },
  methods: {
    getProductById(id) {
      fetch(api_url("/products/" + id))
        .then((result) => result.json())
        .then((data) => (this.product = data));
    },
    addtoCart() {
      fetch(api_url("/cart/"), {
        method: "POST",
        body: JSON.stringify({
          productId: this.product.id,
          productName: this.product.name,
          quantity: 1,
          productPrice: this.product.price,
        }),
        headers: {
          "Content-Type": "application/json",
          // 'Content-Type': 'application/x-www-form-urlencoded',
        },
      });
    },
  },
};
</script>