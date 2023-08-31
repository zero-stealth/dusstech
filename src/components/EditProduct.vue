<template>
    <div class="form-container-h">
      <h1>Product updates</h1>
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
const emit = defineEmits(['formSubmitProduct'])

const Name = ref('')
const Quantity = ref('')
const ProductPrice = ref('')
const ProductImage = ref(null);
const ProductDescription = ref('')

function handleFileUpload(event, targetRef) {
  const file = event.target.files[0]
  if (file) {
    targetRef.value = file
  }
}

function handleProductImage(event) {
  handleFileUpload(event, ProductImage)
}


function handleSubmit() {
  try {
    const formData = {
      name: Name.value,
      quantity: Quantity.value,
      image: ProductImage.value,
      price: ProductPrice.value,
      description: ProductDescription.value,
    }

    emit('formSubmitProduct', formData)
  } catch (err) {
    console.log(err)
  }
}
</script>

<style>
@import '../style/games.css';
</style>
