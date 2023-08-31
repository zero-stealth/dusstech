<template>
  <div class="auth-container" >
    <div class="form-l-wrapper">
      <h1>Create an account</h1>
      <form @submit.prevent="create" class="l-form">
        <input type="email" class="input-l" placeholder="email address" v-model="email" />
        <input type="text" class="input-l" placeholder="phone number" v-model="phoneNumber" />
        <input type="password" class="input-l" placeholder="password 8 character (capital,lowercase,number)" v-model="password" />
        <input type="password" class="input-l" placeholder="confirm Password" v-model="confirmPassword" />
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
const password = ref('')
const errMsg = ref('')
const email = ref('')
const phoneNumber = ref('')
const confirmPassword = ref('')

const reset = () => {
  email.value = ''
  password.value = ''
  phoneNumber.value = ''
  confirmPassword.value = ''
}
const serverHost = import.meta.env.VITE_SERVER_HOST

const create = async () => {
  if (email.value !== '' && password.value !== '') {
    try {
      const response = await axios.post(`${serverHost}/api/v1/auth/register-admin`, {
        email: email.value,
        password: password.value,
        phoneNumber: phoneNumber.value,
        confirmPassword: confirmPassword.value,
      })
      const token = response.data.token
      const email = response.data.email
      localStorage.setItem('email', email)
      localStorage.setItem('token', JSON.stringify(token))

      router.push({ name: 'Panel' })
    } catch (error) {
    errMsg.value = 'Password must contain uppercase, number and special characters'

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
