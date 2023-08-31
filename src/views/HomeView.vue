<script setup>
import { onMounted, ref } from 'vue'
import { useRouter } from 'vue-router'
import AboutView from './AboutView.vue';
import Portfolio from './PortfolioView.vue';
import ArrowIcon from '../icons/arrowIcon.vue';
import { Application } from '@splinetool/runtime';
import Trending from '../components/TrendingCard.vue';


const router = useRouter()

const urls = [
  'https://prod.spline.design/Bia-7wCvBodRGMig/scene.splinecode', 
  'https://prod.spline.design/J2HyNf3VRYaO048e/scene.splinecode',
  'https://prod.spline.design/KelQCHw0AUIf25vN/scene.splinecode',
  'https://prod.spline.design/08Y-hWmt4dWRBNFk/scene.splinecode',
  'https://prod.spline.design/qMkI1MQw7qVwtjAq/scene.splinecode',
  'https://prod.spline.design/vyYWtH9moGpgogEq/scene.splinecode'
]

const currentIndex = ref(0)

onMounted(() => {
  const canvas = document.getElementById('canvas3d')
  if (canvas) {
    const spline = new Application(canvas)
    setInterval(() => {
      const newIndex = (currentIndex.value + 1) % urls.length
      currentIndex.value = newIndex
      spline.load(urls[newIndex])
    }, 10000) // 10 seconds interval
  } else {
    console.error('Canvas element not found.')
  }
})

const goLogin = () => {
  router.push({ name: 'Login'})
}

const goPortfolio = () => {
  router.push({ name: 'Portfolio'})
}
</script>

<template>
  <div class="home-wrap">
    <div class="home-container">
    <div class="hero-section">
      <div class="hero-ban">
        <span></span>
        <h2>A trusted design agency</h2>
      </div>
      <h1>Stand out from the crowd with creative design and branding</h1>
      <p>
        Make a lasting impression with our innovative and eye-catching designs that will set your
        brand apart from the competition.
      </p>
      <button class="hero-btn btn-a" @click="goLogin()">
        Get started
        <ArrowIcon class="arrow-icon" />
      </button>
      <button class="hero-btn btn-b" @click="goPortfolio()">
        See portfolio
        <ArrowIcon class="arrow-icon" />
      </button>
    </div>
    <div class="hero-s">
      <canvas id="canvas3d" class="canvas-s"></canvas>
     <div class="hero-span-contain">
      <div class="hero-span">
        <div class="span-h">
          <h1>12M+</h1>
          <span>no of clients</span>
        </div>
        <div class="span-h">
          <h1>101M+</h1>
          <span>no of projects</span>
        </div>
        <div class="span-h">
          <h1>10Y+</h1>
          <span>Experience</span>
        </div>
      </div>
     </div>
    </div>
  </div>
  <AboutView/>
  <Trending/>
  <div>
    <Portfolio/>
  </div>
  </div>
</template>
<style scoped>
@import '../style/home.css';
</style>
