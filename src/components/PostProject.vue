<template>
  <div>
    <div class="form-container-h">
      <h1>Post Projects</h1>
    </div>
    <form @submit.prevent="handleSubmit" enctype="multipart/form-data" class="form-container">
      <div class="form-wrapper">
        <div class="form-group">
          <label for="Name">Project Name:</label>
          <input
            v-model="Name"
            type="text"
            class="form-g-input"
            placeholder="logo design"
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
            placeholder="logo design done by Adobe"
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
import axios from 'axios'

const Name = ref('')
const ProjectImage = ref(null)
const ProjectDescription = ref('')


function handleFileUpload(event, targetRef) {
  const file = event.target.files[0]
  if (file) {
    targetRef.value = file
  }
}

function handleProjectImage(event) {
  handleFileUpload(event, ProjectImage)
}

const serverHost = import.meta.env.VITE_SERVER_HOST

async function handleSubmit() {
  if (
    Name.value.trim() !== '' &&
    ProjectImage.value !== null &&
    ProjectDescription.value.trim() !== ''
  ) {
    const user = JSON.parse(localStorage.getItem('token'))
    try {
      const formData = new FormData()
      formData.append('title', Name.value)
      formData.append('image', ProjectImage.value)
      formData.append('description', ProjectDescription.value)

      const response = await axios.post(`${serverHost}/api/v1/work/add`, formData, {
        headers: {
          'Content-Type': 'multipart/form-data',
          Authorization: `Bearer ${user}`
        }
      })
      alert('project posted')
    } catch (err) {}
  } else {
    alert('No empty fields allowed')
  }
}
</script>

<style>
@import '../style/games.css';
</style>
