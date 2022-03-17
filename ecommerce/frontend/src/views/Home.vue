<template>
  <div class="home">
    <div>
      <template>
        <h1>Listado de productos</h1>
        <div class="products" style="display: grid; grid-template-columns: 500px 500px; margin-left: 150px;">
            <div v-for="product, index in products" v-bind:key="index" style="margin-top: 50px; margin-left: 50px;">
                <div class="imagenYtexto" style="display:flex;">
                    <div class="imagen" style="margin-left: 20px; margin-top: 15px;">
                        <img :src="product.mainImage" alt="holaxd" width="250" height="250">
                    </div>
                    <div class="nombreYprecio">
                        <div class="idYnombre" style="text-align: left; margin-left: 20px; margin-top: 15px;">
                            ID: {{index}} - {{product.name}}
                        </div>
                        <div class="descripcion" style="text-align: left; margin-left: 20px; margin-top: 15px;">
                            {{product.price}}€
                        </div>
                    </div>
                </div>
                <div class="datos">
                    <div class="descripcion" style="text-align: justify; margin-left: 20px; margin-top: 15px;">
                        {{product.description}} <br>
                    </div>
                    <b-button @click="addCarrito(product.id, product.name, product.price)">Add</b-button>
                </div> 
            </div>
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
    addCarrito(productId, productName, productPrice) {
      fetch(api_url("/cart/"), {
        method: "POST",
        body: JSON.stringify({
          productId: productId,
          productName: productName,
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
