<script setup>
import { ref } from 'vue'

const { subtotal, productName, productPrice } = defineProps([
  'subtotal',
  'productName',
  'productPrice'
])

const phoneNumber = ref('254722277778')
const message = ref('Hi, can I get this order')
const btnTitle = ref('order now')


const pay = () => {
  window.open(
    `https://wa.me/${phoneNumber.value}?text=
      *productName*: ${productName}
      *productPrice*: ${productPrice}
      *Message*: ${message.value}`,
    '_blank'
  )
}
</script>
<template>
  <div class="checkout-container">
    <div class="checkinfo"></div>
    <div class="checkinfo">
      <div>
        <h1>Subtotal:</h1>
      </div>
      <span>{{ subtotal }}</span>
    </div>
    <div class="checkinfo">
      <div>
        <h1>Total:</h1>
      </div>
      <span>{{ subtotal }}</span>
    </div>
    <div class="checkout-btn-contain">
      <button class="checkout-btn" @click="pay()">{{ btnTitle }}</button>
    </div>
  </div>
</template>
<style>
@import '@/style/checkout.css';
</style>
