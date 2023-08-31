<script setup>
import { useRouter } from 'vue-router'
import { ref, onMounted } from 'vue'
import axios from 'axios'

const router = useRouter()
const data = ref([])

const shop = (cartID) => {
  router.push({ name: 'Cart', params: { cartID: cartID } })
}
const serverHost = import.meta.env.VITE_SERVER_HOST

const getCard = async () => {
  try {
    const response = await axios.get(
      `${serverHost}/api/v1/products`
    )
    data.value = response.data
  } catch (err) {
    console.log(err)
  }
}

onMounted(() => {
  getCard()
})
</script>
<template>
  <template v-if="data.length > 0">
    <div class="trending-card">
      <div class="collection-header">
        <h1>Buy our clothes and merch</h1>
      </div>
      <div class="collection-wrapper">
        <div
          class="trending-c"
          v-for="({ _id, image, name, quantity, price ,  description}, index) in data"
          :key="index"
        >
          <img :src="image" :alt="name" class="card-b-img" />
          <div class="card-info">
            <div class="quantity-p">
              <h1>Quantity : <span>{{ quantity }}</span></h1>
            </div>
            <h1>Name : <span>{{ name }}</span></h1>
            <h1>Price : <span>ksh {{ price }}</span></h1>
           <p>{{description}}</p>
            <button @click="shop(_id)" class="product-btn">order now</button>
          </div>
        </div>
      </div>
    </div>
  </template>
  <template v-else>
    <div class="trending-card t-c">
      <h1>Shop empty 🛍</h1>
    </div>
  </template>
</template>
<style scoped>
@import '@/style/collection.css';
@import '@/style/trending.css';
</style>
