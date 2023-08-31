<script setup>
import logo from '../assets/logo.png'
import profile from '../assets/profile.jpg'
import arrowIcon from '../icons/arrowIcon.vue'
import { RouterLink, useRouter } from 'vue-router'

const router = useRouter()


const openTwitter = () => {
  window.open('https://wa.me/254722277778?text=Hi Welcome to dusstech', '_blank')
}

const openFacebook = () => {
  window.open('https://wa.me/254722277778?text=Hi Welcome to dusstech', '_blank')
}

const openInstagram = () => {
  window.open('https://wa.me/254722277778?text=Hi Welcome to dusstech', '_blank')
}

const openWhatsapp = () => {
  window.open('https://wa.me/254722277778?text=Hi Welcome to dusstech', '_blank')
}

const Booknow = () => {
  window.open('https://wa.me/254722277778?text=Hi Welcome to dusstech, start booking', '_blank')
}

const goP = () => {
  router.push({ name: 'AdminLogin'})
}


</script>
<template>
  <div class="contact-main" id="footer">
    <div class="testimony">
      <div class="testimony-info">
        <p>
          I really enjoyed working with you, your designs are perfect. Hope to work with you again
        </p>
      </div>
      <div class="testimony-profile">
        <img :src="profile" alt="profile" class="profile-img" />
        <div class="profile-info">
          <h1>Chris sakwa</h1>
          <p>Business Man</p>
        </div>
      </div>
    </div>
    <div class="contact-footer">
      <div class="footer-h">
        <h1>Want a design ?</h1>
        <button class="footer-btn" @click="Booknow()">
          Book Now
          <arrowIcon class="footer-i" />
        </button>
      </div>
      <hr />
      <div class="footer-info">
        <div class="footer-a">
          <img :src="logo" alt="logo" class="f-logo" />
          <p>We provide the best services at affordable prices to ensure your businee suceed</p>
          <span>dusstech@gmail.com</span>
        </div>
        <div class="footer-c">
          <h1>Social links</h1>
          <div class="f-links">
              <li @click="openTwitter">Twitter</li>
              <li @click="openFacebook">Facebook</li>
              <li @click="openInstagram">Instagram</li>
              <li @click="openWhatsapp">Whatsapp</li>
          </div>
        </div>
        <div class="footer-b">
          <h1>Navigation Links</h1>
          <div class="f-links">
            <RouterLink :to="{ name: 'Shop' }" class="f-link"> Shop </RouterLink>
            <RouterLink :to="{ name: 'Home' }" class="f-link"> Home </RouterLink>
            <RouterLink :to="{ name: 'About' }" class="f-link"> About </RouterLink>
            <a href="#footer" class="f-link">Contact</a>
            <RouterLink :to="{ name: 'Portfolio' }" class="f-link"> Our works </RouterLink>
          </div>
        </div>
      </div>
      <div class="foot-a-info">
        <span>All right reserved by <span @click="goP()">©</span> dusstechbrands</span>
      </div>
    </div>
  </div>
</template>
<style>
@import '../style/footer.css';
</style>
