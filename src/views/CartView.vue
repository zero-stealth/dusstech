<template>
  <div>
    <template v-if="Name == ''">
      <div class="cart-nothing">
        <h1>Nothing in cart 😇</h1>
      </div>
    </template>
    <template v-else>
      <div class="cart-contain">
        <div class="cart-layout">
        <div class="cart-lay1" :style="{ backgroundImage: `url(${image})` }">
        </div>
          <div class="cart-lay2">
            <div class="card-info">
            <h1>Name : <span>{{ Name }}</span></h1>
            <h1>Price : <span>ksh {{ price }}</span></h1>
          </div>
            <CheckOut :subtotal="price" :productName="Name" :productPrice="price" class="show-ds" />
          </div>
        </div>
          <CheckOut :subtotal="price" :productName="Name" :productPrice="price" class="show-mb" />
      </div>
    </template>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';
import CheckOut from '@/components/CheckOut.vue';

const route = useRoute();
const param = route.params.cartID;
const Name = ref('');
const price = ref('');
const quantity = ref('');
const description = ref('');
const image = ref(null);
const itemNo = ref(1);

const serverHost = import.meta.env.VITE_SERVER_HOST;

const getCart = async () => {
  try {
    const response = await axios.get(`${serverHost}/api/v1/products/${param}`);
    image.value = response.data.image;
    Name.value = response.data.name;
    price.value = response.data.price;
    quantity.value = response.data.quantity;
    description.value = response.data.description;
  } catch (err) {
    console.log(err);
  }
};

onMounted(() => {
  getCart();
});

</script>

<style>
@import '@/style/cart.css';
</style>
