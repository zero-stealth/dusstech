<template>
  <div class="auth-container" >
    <div class="form-l-wrapper">
      <h1>Create an account</h1>
      <form @submit.prevent="create" class="l-form">
        <input type="text" class="input-l" placeholder="full name" v-model="username" />
        <input type="text" class="input-l" placeholder="phone number" v-model="phoneNumber" />
        <input type="email" class="input-l" placeholder="email address" v-model="email" />
        <input type="password" class="input-l" placeholder="8 character (special,lowercase,number)" v-model="password" />
        <input type="password" class="input-l" placeholder="8 character (special,lowercase,number)" v-model="confirmPassword" />
        <p>{{ errMsg }}</p>
        <button class="btn-f" type="submit">Sign up</button>
      </form>
      <span>or</span>
      <div class="l-alternatives">
        <button class="alt-btn" @click="login">
          Login
        </button>
        <span @click="goHome()" class="reverse">Go back home</span> 
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
import { useRouter } from 'vue-router'

const router = useRouter()
const username = ref('')
const password = ref('')
const errMsg = ref('')
const email = ref('')
const phoneNumber = ref('')
const confirmPassword = ref('')

const reset = () => {
  email.value = ''
  password.value = ''
  username.value = ''
  phoneNumber.value = ''
  confirmPassword.value = ''
}
const serverHost = import.meta.env.VITE_SERVER_HOST

const create = async () => {
  if (username.value !== '' && password.value !== '') {
    try {
      const response = await axios.post(`${serverHost}/api/v1/auth/register-admin`, {
        username: username.value,
        email: email.value,
        password: password.value,
        phoneNumber: phoneNumber.value,
      })
      console.log(response.data) // Handle the response data as needed
      const token = response.data.token
      const id = response.data._id
      
      localStorage.setItem('username', username)
      localStorage.setItem('token', JSON.stringify(token))
      localStorage.setItem('id', id)

      router.push({ name: 'Panel' })
    } catch (error) {
      console.error(error)
    }
  } else {
    errMsg.value = 'Write something'
    reset()
  }
}


const login = () => {
  router.push({ name: 'AdminLogin' })
}

const goHome = () => {
  router.push({ name: 'Home' })
}
</script>

<style>
@import '../style/auth.css';
</style>
