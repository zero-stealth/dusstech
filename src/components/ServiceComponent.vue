<template>
  <div class="service-header">
    <div class="service-h1">
      <h1>We offer several services for your benefit</h1>
    </div>
    <div class="service-p">
      <p>
        We are passionate about providing the best possible experience to our customers, ensuring
        their satisfaction and building lasting relationships
      </p>
    </div>
  </div>
  <div class="service">
    <div class="selected-service">
      <div v-if="selectedService">
        <img :src="selectedService.image" :alt="selectedService.name" class="service-img-i" />
      </div>
      <div v-else>
        <img :src="defaultImage" alt="Default Service" class="service-img-i" />
      </div>
    </div>
    <div class="service-list">
      <div
        v-for="serviceItem in services"
        :key="serviceItem.name"
        class="service-item"
        :class="{
          'selected-service-b': selectedService && selectedService.name === serviceItem.name
        }"
        @click="selectService(serviceItem)"
      >
        <h2
          :class="{
            'selected-service-name': selectedService && selectedService.name === serviceItem.name
          }"
        >
          {{ serviceItem.name }}
        </h2>
        <p>{{ serviceItem.description }}</p>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref } from 'vue'

import branding from '../assets/branding.png'
import design from '../assets/design.jpg'
import marketing from '../assets/marketing.jpg'
import printing from '../assets/printing.jpg'
import web from '../assets/web.jpg'

const services = [
  {
    name: '01 Graphic Design',
    description:
      'Dusstech Brands has been in the industry long enough to master the different aspects of graphic design and has a 10-year work portfolio to show for it.',
    image: design
  },
  {
    name: '02 Printing',
    description:
      'Dusstech is well equipped with high-quality printers, plotters, and large format printers that guarantee a clean and professional experience.',
    image: printing
  },
  {
    name: '03 Branding',
    description:
      'From company creation to launching, Dusstech brands are with you all through providing a seamless experience in all branding fields.',
    image: branding
  },
  {
    name: '04 Web Development',
    description:
      "Dusstech deals with modern web development trends to create custom web systems that fit the clients' needs.",
    image: web
  },
  {
    name: '05 Digital Marketing',
    description:
      'Dusstech is well equipped with experts that provide professional digital marketing assistance.',
    image: marketing
  }
]

const selectedService = ref({
  name: '01 Graphic Design',
  description:
    'Dusstech Brands has been in the industry long enough to master the different aspects of graphic design and has a 10-year work portfolio to show for it.',
  image: design
})
const defaultImage = branding

function selectService(service) {
  selectedService.value = service
}
</script>

<style scoped>
@import '../style/service.css';
</style>
