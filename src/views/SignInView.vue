<template>
  <div
    class="auth-container">
    <div class="form-l-wrapper">
      <h1>Create an account</h1>
      <form @submit.prevent="create" class="l-form">
        <input type="email" class="input-l" placeholder="Email Address" v-model="email" />
        <input type="password" class="input-l" placeholder="Password" v-model="password" />
        <input type="password" class="input-l" placeholder="Confirm password" v-model="confirmPassword" />
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
const confirmPassword = ref('')

const reset = () => {
  email.value = ''
  password.value = ''
  confirmPassword.value = ''
}

const serverHost = import.meta.env.VITE_SERVER_HOST

const create = async () => {
  if (username.value !== '' && password.value !== '') {
    try {
      const response = await axios.post(`${serverHost}/api/v1/auth/register`, {
        username: username.value,
        email: email.value,
        password: password.value,
      })
      const token = response.data.token
      const id = response.data._id
      localStorage.setItem('token', token)
      localStorage.setItem('id', id)
      router.push({ name: 'Vip' })
    } catch (error) {
      errMsg.value = 'Invalid email or password';
      alert(errMsg.value)
    }
  } else {
    errMsg.value = 'Write something'
    alert(errMsg.value)
    reset()
  }
}
const login = () => {
  router.push({ name: 'Login' })
}

const goHome = () => {
  router.push({ name: 'Home' })
}
</script>

<style>
@import '../style/auth.css';
</style>
