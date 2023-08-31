<template>
  <div class="Account-container">
    <div class="Account-header">
      <h1><span>Welcome</span><br />Dusstech</h1>
    </div>
    <div class="Account-game-p">
      <!-- loop this  -->
      <div class="acc-m gm-m">
        <div class="main-header">
          <div class="header-info">
            <h1>Product</h1>
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
          <template class="t-c" v-if="ProductData.length > 0">
            <h1>No Product Found</h1>
          </template>
          <template v-else>
            <tbody v-for="item in ProductData" :key="item">
              <tr v-for="data in item" :key="data._id">
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
              <tr v-if="cardData.length === 0">
                <td colspan="8">No data</td>
              </tr>
            </tbody>
          </template>
        </table>
      </div>
      <!-- loop this  -->
      <div class="acc-m gm-m">
        <div class="main-header">
          <div class="header-info">
            <h1>Project</h1>
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
          <template class="t-c" v-if="ProjectData.length > 0">
            <h1>No Product Found</h1>
          </template>
          <template>
            <tbody v-for="item in ProjectData" :key="item">
              <tr v-for="data in item" :key="data._id">
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
              <tr v-if="cardData.length === 0">
                <td colspan="8">No data</td>
              </tr>
            </tbody>
          </template>
        </table>
      </div>
      <!-- loop this  -->
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
import EditProject from '../components/EditProject.vue'
import EditProduct from '../components/EditProduct.vue'

const message = ref()
const isGameOpen = ref(false)
const ProjectData = ref([])
const ProductData = ref([])
const serverHost = import.meta.env.VITE_SERVER_HOST

const getProject = async () => {
  try {
    // const token = JSON.parse(localStorage.getItem('token'));
    const response = await axios.get(`${serverHost}/api/v1/work`)
    console.log(response.data)
    ProjectData.value = response.data
    // ProjectData.value = response.data.length > 0 ? [response.data] : []
  } catch (err) {
    console.log(err)
  }
}

const getProduct = async () => {
  try {
    const response = await axios.get(`${serverHost}/api/v1/products`)
    console.log(response.data)
    // ProductData.value = response.data.length > 0 ? [response.data] : []
    ProductData.value = response.data
  } catch (err) {
    console.log(err)
  }
}

const showEdit = () => {
  isGameOpen.value = !isGameOpen.value
}

const activePage = shallowRef(EditProject)
const ProductID = ref('')
const WorkID = ref('')

const editData = (data, id) => {
  activePage.value = data
  ProductID.value = id
  showEdit()
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
      `${serverHost}/api/v1/work/update/${WorkID.value}`,
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
      `${serverHost}/api/v1/products/update/${ProductID.value}`,
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
  } catch (err) {
    message.value = 'deletion failed'
  }
  alert('deleted')
}
const deleteProduct = async (id) => {
  try {
    const token = JSON.parse(localStorage.getItem('token'))

    const response = await axios.delete(`${serverHost}/api/v1/products/delete/${id}`,{
        headers: { Authorization: `Bearer ${token}` }
      }
    )

    await getProduct()
  } catch (err) {
    message.value = 'deletion failed'
  }
  alert('deleted')
}
</script>

<style>
@import '../style/account.css';
@import '../style/Bet.css';
</style>
