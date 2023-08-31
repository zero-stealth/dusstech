<script setup>
import { RouterView } from 'vue-router'
import NavBar from './components/NavComponent.vue'
import Footer from './components/FooterComponent.vue'
</script>
<template>
  <div class="app-main">
    <NavBar />
    <RouterView />
    <div>
      <Footer />
    </div>
  </div>
</template>
<style>
@import './style/app.css';
</style>
