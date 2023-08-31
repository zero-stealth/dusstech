<template>
  <div class="Account-container">
    <div class="Account-header">
      <h1><span>Welcome</span><br />Dusstech</h1>
    </div>
    <div class="Account-game-p">
      <div class="acc-m">
        <div class="main-header">
          <div class="header-info">
            <h1>Products Available</h1>
          </div>
        </div>
        <table>
          <thead>
            <tr>
              <th>Image</th>
              <th>Name</th>
              <th>Price</th>
              <th>Edit</th>
              <th>Delete</th>
            </tr>
          </thead>
          <tbody v-if="ProductData.length === 0">
            <tr>
              <td colspan="5">No Product Found</td>
            </tr>
          </tbody>
          <tbody v-else>
            <tr v-for="data in ProductData" :key="data._id">
              <td>
                <div class="Account-tbl-img">
                  <img :src="data.image" alt="Account-p" class="Account-pi" />
                </div>
              </td>
              <td>
                <span>{{ data.name }}</span>
              </td>
              <td>
                <span>{{ data.price }}</span>
              </td>
              <td>
                <div class="Account-delete" @click="editData(EditProduct, data._id)">
                  <FileIcon class="icon-delete" />
                </div>
              </td>
              <td>
                <div class="Account-delete" @click="deleteProduct(data._id)">
                  <DeleteIcon class="icon-delete" />
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="acc-m">
        <div class="main-header">
          <div class="header-info">
            <h1>Projects Available</h1>
          </div>
        </div>
        <table>
          <thead>
            <tr>
              <th>Image</th>
              <th>Name</th>
              <th>Edit</th>
              <th>Delete</th>
            </tr>
          </thead>
          <tbody v-if="ProjectData.length === 0">
            <tr>
              <td colspan="4">No Project Found</td>
            </tr>
          </tbody>
          <tbody v-else>
            <tr v-for="data in ProjectData" :key="data._id">
              <td>
                <div class="Account-tbl-img">
                  <img :src="data.image" alt="Account-p" class="Account-pi" />
                </div>
              </td>
              <td>
                <span>{{ data.title }}</span>
              </td>
              <td>
                <div class="Account-delete" @click="editData(EditProject, data._id)">
                  <FileIcon class="icon-delete" />
                </div>
              </td>
              <td>
                <div class="Account-delete" @click="deleteProject(data._id)">
                  <DeleteIcon class="icon-delete" />
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
  <Teleport to="body">
    <div class="game-edit" :class="[isGameOpen ? 'showEdit' : 'closeEdit']">
      <div class="mobile-exit">
        <ExitIcon class="icon-exit" @click="showEdit()" />
      </div>
      <div class="game-main-p">
        <component
          @formSubmit="updateProject"
          @formSubmitProduct="updateProduct"
          :is="activePage"
        />
      </div>
    </div>
  </Teleport>
</template>

<script setup>
import axios from 'axios'
import { ref, onMounted, shallowRef } from 'vue'
import ExitIcon from '../icons/exitIcon.vue'
import FileIcon from '../icons/fileIcon.vue'
import DeleteIcon from '../icons/delete.vue'
import EditProject from './EditProduct.vue'
import EditProduct from './EditProduct.vue'
import { Teleport } from 'vue'

const isGameOpen = ref(false)
const ProjectData = ref([])
const ProductData = ref([])
const serverHost = import.meta.env.VITE_SERVER_HOST

const getProject = async () => {
  try {
    const response = await axios.get(`${serverHost}/api/v1/work`)
    ProjectData.value = response.data
  } catch (err) {
    console.log(err)
  }
}

const getProduct = async () => {
  try {
    const response = await axios.get(`${serverHost}/api/v1/products`)
    ProductData.value = response.data
  } catch (err) {
    console.log(err)
  }
}

const showEdit = () => {
  isGameOpen.value = !isGameOpen.value
}

const activePage = shallowRef(EditProject)
const DataID = ref('')

const editData = (items, id) => {
  activePage.value = items
  DataID.value = id
}

async function updateProduct(formData) {
  try {
    const token = JSON.parse(localStorage.getItem('token'))
    const formDataa = new FormData()
    if (formData.Name !== '') {
      formDataa.append('name', formData.Name)
    }
    if (formData.Quantity !== '') {
      formDataa.append('quantity', formData.Quantity)
    }
    if (formData.ProductPrice !== '') {
      formDataa.append('price', formData.ProductPrice)
    }
    if (formData.ProjectDescription !== '') {
      formDataa.append('description', formData.ProjectDescription)
    }
    if (formData.ProductImage !== null) {
      formDataa.append('image', formData.ProductImage)
    }
    const response = await axios.put(
      `${serverHost}/api/v1/work/update/${DataID.value}`,
      formDataa,
      {
        headers: {
          Authorization: `Bearer ${token}`,
          'Content-Type': 'multipart/form-data'
        }
      }
    )
    console.log(response.data)
    alert('Product updated')
  } catch (error) {
    console.error('Error updating:', error)
  }
}

async function updateProject(formData) {
  try {
    const token = JSON.parse(localStorage.getItem('token'))
    const formDataa = new FormData()
    if (formData.Name !== '') {
      formDataa.append('title', formData.Name)
    }
    if (formData.ProjectDescription !== '') {
      formDataa.append('description', formData.ProjectDescription)
    }
    if (formData.ProjectImage !== null) {
      formDataa.append('image', formData.ProjectImage)
    }
    const response = await axios.put(
      `${serverHost}/api/v1/products/update/${DataID.value}`,
      formDataa,
      {
        headers: {
          Authorization: `Bearer ${token}`,
          'Content-Type': 'multipart/form-data'
        }
      }
    )
    console.log(response.data)
    alert('Project updated')
  } catch (error) {
    console.error('Error updating:', error)
  }
}

onMounted(() => {
  getProduct()
  getProject()
})

const deleteProject = async (id) => {
  try {
    const token = JSON.parse(localStorage.getItem('token'))

    const response = await axios.delete(`${serverHost}/api/v1/work/delete/${id}`, {
      headers: { Authorization: `Bearer ${token}` }
    })

    await getProject()
    alert('Deleted')
  } catch (err) {
    console.error('Deletion failed:', err)
  }
}

const deleteProduct = async (id) => {
  try {
    const token = JSON.parse(localStorage.getItem('token'))

    const response = await axios.delete(`${serverHost}/api/v1/products/delete/${id}`, {
      headers: { Authorization: `Bearer ${token}` }
    })

    await getProduct()
    alert('Deleted')
  } catch (err) {
    console.error('Deletion failed:', err)
  }
}
</script>

<style>
@import '../style/account.css';
@import '../style/Bet.css';
</style>
