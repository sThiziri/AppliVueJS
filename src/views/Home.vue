<template>
  <AppBar :shopping-cart="shoppingCart" @incrementQuantityCart="incrementQuantityCart"
    @decrementQuantityCart="decrementQuantityCart" />
  <v-card>
    <v-tabs v-model="tab" color="deep-purple-accent-4" align-tabs="center">
      <v-tab value="Accueil">Accueil</v-tab>
      <v-tab value="Nos poké">Nos poké</v-tab>
      <v-tab value="Desserts">Nos desserts</v-tab>
    </v-tabs>
    <v-card-text>
      <v-window v-model="tab">
        <v-window-item value="Accueil">
          <v-img src="/src/assets/presentation.png">
            <div class="container">
              <span class="txt">BIENVENUE CHEZ POKEBIO</span>
            </div>
          </v-img>
        </v-window-item>

        <v-window-item value="Nos poké">
          <div class="d-flex">
            <Product class="vertical-layout" v-for="(plat, index) in plats" :key="index" :details="plat"
              @incrementQuantity="handleIncrementQuantityPoke" @decrementQuantity="handleDecrementQuantityPoke" />
          </div>
        </v-window-item>

        <v-window-item value="Desserts">
          <div class="d-flex">
            <Product class="vertical-layout" v-for="(dessert, index) in desserts" :key="index" :details="dessert"
              @incrementQuantity="handleIncrementQuantityDessert" @decrementQuantity="handleDecrementQuantityDessert" />
          </div>
        </v-window-item>

          
        

      </v-window>
    </v-card-text>
  </v-card>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import AppBar from '@/components/AppBar.vue';
import Product from '@/components/Product.vue';

let shoppingCart: any = ref({ numberOfProducts: 0, totalCost: 0, productsList: [] });
let tab: any = ref(null);

let plats: any = ref([
  { Id: 1, name: "Poke saumon", description: "saumon, avocat, edamames, choux rouges, concombres et mangue.", imgLink: "/src/assets/saumon.jpg", price: 12, quantity: 0, type: "plat" },
  { Id: 2, name: "Poke thon", description: "thon rouge, avocat, edamames, choux rouges, concombres et mangue.", imgLink: "/src/assets/thon.jpg", price: 12, quantity: 0, type: "plat" },
  { Id: 3, name: "Poke crevettes", description: "crevettes, avocat, edamames, choux rouges, concombres et mangue.", imgLink: "/src/assets/crevette.jpg", price: 12, quantity: 0, type: "plat" },
  { Id: 4, name: "Poke falafel", description: "Fafalel de pois chiche, choux rouges, édamames, carottes râpées, radis, et grenades.", imgLink: "/src/assets/falafel.jpg", price: 12, quantity: 0, type: "plat" }
]);
let desserts: any = ref([
  { Id: 5, name: "Gaufre", description: "farine, sucre, beurre, oeufs, lait, chocolat", imgLink: "/src/assets/gaufre.jpg", price: 6, quantity: 0, type: "dessert" },
  { Id: 6, name: "Glace", description: "sucre, oeuf, crème liquIde, sucre vanillé", imgLink: "/src/assets/glace.jpeg", price: 6, quantity: 0, type: "dessert" },
  { Id: 7, name: "Cupcake", description: "sucre, oeuf, farine, beurre", imgLink: "/src/assets/cupcake.jpg", price: 6, quantity: 0, type: "dessert" },
  { Id: 8, name: "Donuts", description: "sucre, farine, beurre, oeufs, lait", imgLink: "/src/assets/donuts.jpg", price: 6, quantity: 0, type: "dessert" }
]);

const handleIncrementQuantityPoke = (data: any) => {
  plats.value.forEach((el: any) => {
    if (el.Id === data.productId) {
      el.quantity++;
      // console.log(el.quantity);
    }
  });
  updateShoppingCart();
}

const handleDecrementQuantityPoke = (data: any) => {
  plats.value.forEach((el: any) => {
    if (el.Id === data.productId && el.quantity > 0) {
      el.quantity--;
      // console.log(el.quantity);
    }
  });
  updateShoppingCart();
}

const handleIncrementQuantityDessert = (data: any) => {
  desserts.value.forEach((el: any) => {
    if (el.Id === data.productId) {
      el.quantity++;
      // console.log(el.quantity);
    }
  });
  updateShoppingCart();
}

const handleDecrementQuantityDessert = (data: any) => {
  desserts.value.forEach((el: any) => {
    if (el.Id === data.productId && el.quantity > 0) {
      el.quantity--;
      // console.log(el.quantity);
    }
  });
  updateShoppingCart();
}

const updateShoppingCart = () => {
  let products = [...plats.value, ...desserts.value];
  shoppingCart.value = { numberOfProducts: 0, totalCost: 0, productsList: [] };
  products.forEach((el: any) => {
    if (el.quantity > 0) {
      shoppingCart.value.numberOfProducts += el.quantity;
      shoppingCart.value.totalCost += (el.quantity * el.price);
      shoppingCart.value.productsList.push(el);
    }
  });

}
const incrementQuantityCart = (data: any) => {
  if (data.productType === 'plat') {
    handleIncrementQuantityPoke(data);
  } else {
    handleIncrementQuantityDessert(data);
  }
}
const decrementQuantityCart = (data: any) => {
  if (data.productType === 'plat') {
    handleDecrementQuantityPoke(data);
  } else {
    handleDecrementQuantityDessert(data);
  }
}
</script>

<script lang="ts">
export default {
  emits: ['incrementQuantity', 'decrementQuantity', 'incrementQuantityCart', 'decrementQuantityCart']// Define the custom event that the child component can emit
};
</script>

<style>
.container {
  height: 100vh;
  width: 100%;
  display: flex;
  align-items: center;
}

.txt {
  white-space: nowrap;
  font-size: 50px;
  animation: Scroll 8s linear infinite;
  color: antiquewhite;
  text-align: center;
  margin: 70%;
}

@keyframes Scroll {
  0% {
    transform: translate(0%);
  }

  100% {
    transform: translateX(-100%);
  }
}
</style>


