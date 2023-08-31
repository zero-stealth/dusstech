<template>
  <div class="panel-container">
    <div class="panel-bar" :class="[isOpen != false ? 'open-p' : 'close-p']">
      <div class="panel-exit">
        <ExitIcon class="panel-icon-exit" @click="showMenu()" />
      </div>
      <div class="panel-profile">
        <div class="panel-img" :style="{ backgroundImage: `url(${logo})` }"></div>
        <h1>{{ email }}</h1>
      </div>
      <div class="panel-nav">
        <div class="panel-btn-container">
          <button
            @click="setActivePage(ItemManagement)"
            :class="getButtonClass(ItemManagement)"
          >
            <VipIcon class="icon-panel" />
            <span>Item management</span>
            <ArrowIcon class="icon-panel ap" />
          </button>
          <button @click="setActivePage(PostProjects)" :class="getButtonClass(PostProjects)">
            <projectIcon class="icon-panel" />
            <span> Post Projects </span>
            <ArrowIcon class="icon-panel ap" />
          </button>
          <button @click="setActivePage(PostProducts)" :class="getButtonClass(PostProducts)">
            <projectIcon class="icon-panel" />
            <span> Post Products </span>
            <ArrowIcon class="icon-panel ap" />
          </button>
          <button @click="logout" :class="getButtonClass(logout)">
            <LogoutIcon class="icon-panel" />
            <span>Panel logout </span>
            <ArrowIcon class="icon-panel ap" />
          </button>
        </div>
      </div>
    </div>
    <div class="panel-main">
      <div class="panel-menu" @click="showMenu()">
        <homeIcon class="panel-icon-nav" />
      </div>
      <component :is="activePage" />
    </div>
  </div>
</template>
<script setup>
import { ref , watchEffect , shallowRef} from 'vue'
import projectIcon from '../icons/projectIcon.vue'
import LogoutIcon from '../icons/logoutIcon.vue'
import ArrowIcon from '../icons/arrowIcon.vue'
import ExitIcon from '../icons/exitIcon.vue'
import homeIcon from '../icons/homeIcon.vue'
import VipIcon from '../icons/VipIcon.vue'
import logo from '../assets/logo.png'
import { useRouter } from 'vue-router'

const email = ref('admin')
const router = useRouter()
const isOpen = ref(false)

watchEffect(() => {
  email.value = localStorage.getItem('email')
})

const showMenu = () => {
  isOpen.value = !isOpen.value
}
// pages
import ItemManagement from '../components/ItemManagement.vue'
import PostProducts from '../components/PostProducts.vue'
import PostProjects from '../components/PostProject.vue'

const activePage = shallowRef(ItemManagement)

const setActivePage = (page) => {
  activePage.value = page
  showMenu();
}

const getButtonClass = (page) => {
  return ['panel-btn', activePage.value === page ? 'active-cp' : 'inactive-cp']
}

const logout = () => {
  localStorage.removeItem('token');
  localStorage.removeItem('email');
  router.push({ name: 'Home' });
};
</script>
<style>
@import '../style/adminpanel.css';
</style>
