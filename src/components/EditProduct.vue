<template>
  <div>
    <div class="form-container-h">
      <h1>Product updates</h1>
    </div>
    <form @submit.prevent="handleSubmit" enctype="multipart/form-data" class="form-container">
      <div class="form-wrapper">
        <div class="form-group">
          <label for="Title">ProjectPrice:</label>
          <input
            v-model="ProjectPrice"
            type="text"
            class="form-g-input"
            placeholder="10000"
            id="ProjectPrice"
          />
        </div>
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
          <label for="Name">Project Name:</label>
          <input
            v-model="Name"
            type="text"
            class="form-g-input"
            placeholder="branded tshirt"
            id="Name"
          />
        </div>
        <div class="form-group">
          <label for="ProjectImage">Project Image:</label>
          <input
            @change="handleProjectImage"
            type="file"
            class="form-g-input"
            id="ProjectImage"
            accept="image/*"
          />
        </div>
        <div class="form-group">
          <label for="Name">Project Description:</label>
          <textarea
            v-model="ProjectDescription"
            type="text"
            class="form-g-input"
            placeholder="branded tshirt is good"
            id="ProjectDescription"
          />
        </div>
        <button type="submit" class="btn-f-f">Submit</button>
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const emit = defineEmits(['formSubmitProduct'])

const Name = ref('')
const Quantity = ref('')
const ProductPrice = ref('')
const ProjectImage = ref(null);
const ProjectDescription = ref('')

function handleFileUpload(event, targetRef) {
  const file = event.target.files[0]
  if (file) {
    targetRef.value = file
  }
}

function handleProductImage(event) {
  handleFileUpload(event, ProjectImage)
}


function handleSubmit() {
  try {
    const formData = {
      name: Name.value,
      quantity: Quantity.value,
      image: ProjectImage.value,
      price: ProductPrice.value,
      description: ProjectDescription.value,
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
