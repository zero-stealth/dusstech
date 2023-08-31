<script setup>
import { RouterLink, useRouter } from 'vue-router'
import menuIcon from '../icons/menuIcon.vue';
import exitIcon from '../icons/exitIcon.vue';
import logo from '../assets/logo.png'
import { ref } from 'vue'

const router = useRouter()
const isOpen = ref(false)

const Signin = () => {
  router.push({ name: 'Signin'})
}

const showMenu = () => {
  isOpen.value = !isOpen.value
}

</script>
<template>
  <div class="nav-main">
    <div class="nav-container">
      <div class="nav-logo-c">
        <img :src="logo" alt="logo" class="nav-logo" />
      </div>
      <div class="nav-c-m">
        <div class="nav-middle">
          <RouterLink :to="{ name: 'Home' }" class="nav-link"> Home </RouterLink>
          <RouterLink :to="{ name: 'About' }" class="nav-link"> About </RouterLink>
          <RouterLink :to="{ name: 'Portfolio' }" class="nav-link"> Our works </RouterLink>
          <RouterLink :to="{ name: 'Shop' }" class="nav-link"> Shop </RouterLink>
          <!-- <RouterLink :to="{ name: 'Cart', params: { cartID: 'empty' } }" class="nav-link"> Cart </RouterLink> -->
          <a href="#footer" class="nav-link">Contact</a>
        </div>
        <button class="nav-btn" @click="Signin()">Get started</button>
      </div>
      <menuIcon class="menu-icon" @click="showMenu()"/>
    </div>
    <!-- mobile responsive -->
    <div class="mobile-bar" :class="[isOpen ? 'open' : 'close']">
      <div class="mobile-exit">
        <exitIcon class="icon-exit" @click="showMenu()" />
      </div>
      <div class="mobile-container">
        <div class="nav-middle-mb">
          <RouterLink :to="{ name: 'Home' }" class="nav-link"> Home </RouterLink>
          <RouterLink :to="{ name: 'About' }" class="nav-link"> About </RouterLink>
          <RouterLink :to="{ name: 'Shop' }" class="nav-link"> Shop </RouterLink>
          <!-- <RouterLink :to="{ name: 'Cart', params: { cartID: 'empty' } }" class="nav-link"> Cart </RouterLink> -->
          <a href="#footer" class="nav-link">Contact</a>
          <RouterLink :to="{ name: 'Portfolio' }" class="nav-link"> Our works </RouterLink>
        </div>
        <div class="nav-end">
          <button class="mobile-btn btn-ma" @click="Signin()">Sign in</button>
          <button class="mobile-btn btn-mb" @click="Login()">Log in</button>
        </div>
      </div>
      <!-- mobile responsive -->
    </div>
  </div>
</template>
<style scoped>
@import '../style/nav.css';
</style>
