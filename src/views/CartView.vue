
<template>
<template v-if="show">
  <div class="cart-nothing">
    <h1>nothing on cart 😇</h1>
  </div>
</template>
<template v-else>
  <div class="cart-contain">
    <div
      class="cart-layout"
      v-for="({ image, name, price }, index) in filteredProducts"
      :key="index"
    >
      <div class="cart-lay1" :style="{ backgroundImage: `url(${image})` }"></div>
      <div class="cart-lay2">
        <div class="cart-h">
          <h1>{{ name }}</h1>
        </div>
        <div class="sideline">
          <div class="cart-price">
            <span>{{ price }}</span>
          </div>
          <div class="cart-no">
            <div class="inc-contain">
              <div class="cart-in" @click="incrementItemNo()">
                <addIcon class="icon-add" />
              </div>
              <div class="itemNo">{{ itemNo }}</div>
              <div class="cart-in" @click="decrementItemNo()">
                <minusIcon class="icon-minus" />
              </div>
            </div>
            <div class="inc-delete">
              <deleteIcon class="icon-delete" />
            </div>
          </div>
        </div>
        <CheckOut :subtotal="price" :delivery="delivery" :name="name" :price="price" class="show-ds" />
      </div>
    </div>
    <CheckOut :subtotal="subtotal" :delivery="delivery" :name="name" :price="price" class="show-mb" />
  </div>
</template>
</template>
<script setup>
import { ref, watchEffect, onMounted } from 'vue'
import { useRoute } from 'vue-router'
import axios from 'axios'
import addIcon from '@/icons/add.vue'
import minusIcon from '@/icons/minus.vue'
import deleteIcon from '@/icons/delete.vue'
import CheckOut from '@/components/CheckOut.vue'

const route = useRoute()
const show = ref(false)

const param = route.params.cartID
const data = ref([])


watchEffect(() => {
  if (param === 'empty') {
    show.value = !show.value
  } else {
    show.value = false
  }
})

const itemNo = ref(1)

const incrementItemNo = () => {
  itemNo.value++
}

const decrementItemNo = () => {
  if (itemNo.value > 1) {
    itemNo.value--
  } else {
    itemNo.value = 1
  }
}

const serverHost = import.meta.env.VITE_SERVER_HOST

const getCard = async () => {
  try {
    const response = await axios.get(
      `${serverHost}/products/${param}`
    )
    data.value = response.data
  } catch (err) {
    console.log(err)
  }
}

onMounted(() => {
  getCard()
})

// const filteredProducts = computed(() => {
//   return data.filter((d) => d.id.includes(param))
// })

</script>

<style>
@import '@/style/cart.css';
</style>
