<template>
  <div class="auth-container">
    <div class="form-l-wrapper">
      <h1>{{ title }}</h1>
      <form @submit.prevent="login" class="l-form" v-if="!resetPage">
        <input type="email" class="input-l" placeholder="email address" v-model="email" />
        <input type="password" class="input-l" placeholder="Password" v-model="password" />
        <p>{{ errMsg }}</p>
        <button class="btn-f" type="submit">Login</button>
        <span @click="forgot">Forgot password</span>
      </form>
      <form @submit.prevent="resetAuth" class="l-form" v-else>
        <input type="email" class="input-l" placeholder="email address" v-model="email" />
        <input type="password" class="input-l" placeholder="Password" v-model="password" />
        <p>{{ errMsg }}</p>
        <button class="btn-f" type="submit">Reset</button>
      </form>
      <span>or</span>
      <div class="l-alternatives">
        <button class="alt-btn" @click="create">Create an account</button>
        <span @click="goHome()" class="reverse">Go back home</span> 
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
import { useRouter } from 'vue-router'

const resetPage = ref(false)
const router = useRouter()
const password = ref('')
const title = ref('Login')
const errMsg = ref('')
const email = ref('')
const serverHost = import.meta.env.VITE_SERVER_HOST

const login = async () => {
  if (email.value !== '' && password.value !== '') {
    try {
      const response = await axios.post(`${serverHost}/api/v1/auth/login-admin`, {
        email: email.value,
        password: password.value
      })


      const token = response.data.token
      if (token) {
        const email = response.data.email
        localStorage.setItem('email', email)
        localStorage.setItem('token', JSON.stringify(token))

        router.push({ name: 'Panel' })
      } else {
        errMsg.value = 'Invalid email or password'
      }
    } catch (error) {
      errMsg.value = 'Login failed. Please check your email and password.'
    }
  } else {
    errMsg.value = 'Please enter your email and password.'
  }
}

const forgot = () => {
  title.value = 'Reset Your Account'
  resetPage.value = !resetPage.value
}

const goHome = () => {
  router.push({ name: 'Home' })
}

const create = () => {
  router.push({ name: 'AdminSignin' })
}

const resetAuth = async () => {
  if (email.value !== '' && password.value !== '') {
    try {
      const response = await axios.post(`${serverHost}/api/v1/auth/reset`, {
        email: email.value,
        password: password.value
      })
      console.log(response.data)
      resetPage.value = !resetPage.value
    } catch (error) {
      errMsg.value = 'email does not exist'
    }
  } else {
    errMsg.value = 'Write something'
    reset()
  }
}
</script>

<style>
@import '../style/auth.css';
</style>
