<script>
import ModalePanier from "./components/ModalePanier.vue";
import CardProduct from "./components/CardProduct.vue";
import NavBar from "./components/NavBar.vue";

export default {
  name: "App",
  components: {
    NavBar,
    ModalePanier,
    CardProduct,
  },
  data() {
    return { cartItem: [], cartDisplay: false };
  },
  methods: {
    add(item) {
      this.cartItem.push(item);
      this.priceSum;
    },
    openCart() {
      this.cartDisplay = true;
    },
    closeCart() {
      this.cartDisplay = false;
    },
    deleteItem(item) {
      console.log(item);
      let indexToDelete = this.cartItem.indexOf(item);
      this.cartItem.splice(indexToDelete, 1);
    },
   
  },
  computed:{
    priceSum() {
      let total = 0;
      for (let i = 0; i < this.cartItem.length; i++) {
        total += this.cartItem[i][0].price * this.cartItem[i][1];
      }
      return total; 
    },
  }
};
</script>

<template>
  <div>
    <NavBar @openCart="openCart" :cartItem="cartItem" />
    <CardProduct @add="add" />
    <ModalePanier
      :cartItem="cartItem"
      :cartDisplay="cartDisplay"
      @closeCart="closeCart"
      @delete="deleteItem"
      :priceSum="priceSum"
    />
  </div>
</template>

<style></style>
