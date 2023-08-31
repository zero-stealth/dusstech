<template>
    <div class="form-container-h">
      <h1>Post Products</h1>
    </div>
    <form @submit.prevent="handleSubmit" enctype="multipart/form-data" class="form-container">
      <div class="form-wrapper">
        <div class="form-group">
          <label for="Title">Quantity:</label>
          <input
            v-model="Quantity"
            type="text"
            class="form-g-input"
            placeholder="1"
            id="Quantity"
          />
        </div>
        <div class="form-group">
          <label for="Title">Product Price:</label>
          <input
            v-model="ProductPrice"
            type="text"
            class="form-g-input"
            placeholder="10000"
            id="ProductPrice"
          />
        </div>
        <div class="form-group">
          <label for="Name">Product Name:</label>
          <input
            v-model="Name"
            type="text"
            class="form-g-input"
            placeholder="branded tshirt"
            id="Name"
          />
        </div>
        <div class="form-group">
          <label for="ProductImage">Product Image:</label>
          <input
            @change="handleProductImage"
            type="file"
            class="form-g-input"
            id="ProductImage"
            accept="image/*"
          />
        </div>
        <div class="form-group">
          <label for="Name">Product Description:</label>
          <textarea
            v-model="ProductDescription"
            type="text"
            class="form-g-input"
            placeholder="branded tshirt is good"
            id="ProductDescription"
          />
        </div>
        <button type="submit" class="btn-f-f">Submit</button>
      </div>
    </form>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const Name = ref('')
const Quantity = ref('')
const ProductPrice = ref('')
const ProductImage = ref(null);
const ProductDescription = ref('')

const reset = () => {
  Name.value = ''
  Quantity.value = ''
  ProductPrice.value = ''
  ProductImage.value = ''
  ProductDescription.value = ''
}


function handleFileUpload(event, targetRef) {
  const file = event.target.files[0]
  if (file) {
    targetRef.value = file
  }
}

function handleProductImage(event) {
  handleFileUpload(event, ProductImage)
}
const serverHost = import.meta.env.VITE_SERVER_HOST
async function handleSubmit() {
  if (
    Name.value.trim() !== '' &&
    ProductImage.value !== null &&
    Quantity.value.trim() !== ''&&
    ProductPrice.value.trim() !== ''&&
    ProductDescription.value.trim() !== ''
  ) {
    const user = JSON.parse(localStorage.getItem('token'))
    try {
      const formData = new FormData()
      formData.append('name', Name.value)
      formData.append('quantity', Quantity.value)
      formData.append('image', ProductImage.value)
      formData.append('price', ProductPrice.value)
      formData.append('description', ProductDescription.value)

      const response = await axios.post(`${serverHost}/api/v1/products/create`, formData, {
        headers: {
          'Content-Type': 'multipart/form-data',
          Authorization: `Bearer ${user}`
        }
      })
      alert('Product posted')
      reset();
    } catch (err) {}
  } else {
    alert('No empty fields allowed')
  }
}
</script>

<style>
@import '../style/games.css';
</style>
