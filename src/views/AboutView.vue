<template>
  <div class="about-main">
    <div class="img-scope-contain">
      <div class="img-scope">
        <span v-for="(image, index) in images" :key="index" :style="`--i:${index + 1};`">
          <img
            :src="image.url"
            alt="not found"
            @mouseover="hoveredImageIndex = index"
            @mouseout="hoveredImageIndex = null"
          />
        </span>
      </div>
    </div>
    <div class="abt-info">
      <div class="abt-glass">
        <h1>{{ hoveredInfo.type || defaultInfo.type }}</h1>
        <p>{{ hoveredInfo.description || defaultInfo.description }}</p>
      </div>
    </div>
  </div>
  <Service/>
</template>

<script setup>
import Service from '../components/ServiceComponent.vue'
import experience from '../assets/experience.jpg'
import platforms from '../assets/platforms.jpg'
import direction from '../assets/direction.jpg'
import customer from '../assets/customer.png'
import delivery from '../assets/delivery.png'
import { ref, watch } from 'vue';


const images = [
  {
    url: platforms,
    type: 'Digital Platforms',
    description: 'Realisation of your digital aspirations will need agility,capacity and scalabity. We go it covered'
  },
  {
    url: experience,
    type: 'Digital Experience',
    description: 'We help you create that seamless digital experience that generates adoption, engagement and satisfaction.'
  },
  {
    url: direction,
    type: 'Digital Direction',
    description: 'We focus on helping clients navigate the sometimes complex digital landscape and provide expertise that generate value'
  },
  {
    url: customer,
    type: '24/7 Support',
    description: 'Our support staff is always ready to help you out. in case of any queries you can call (+254)722-277-7788'
  },
  {
    url: delivery,
    type: 'Delivery',
    description: 'We deliver country wide when the delivery fee is paid'
  }
  
]

const defaultInfo = {
  type: 'We are a Branding Solution With 10 Years Of Experience',
  description: 'Dusstech Brands is a Kenyan registered creative agency with a diverse portfolio of clients in search of top-notch printing, branding, and marketing services locally and across the border. Equipped with a team of graphic designers, content creators, web developers, and copywriters, we have established Trust, Passion, and Leadership with all our clients in the quest to meet their specific organizational goals.',
};

const hoveredInfo = ref({ type: '', description: '' });
const hoveredImageIndex = ref(null);

// Watch for changes in hoveredImageIndex and update hoveredInfo accordingly
watch(hoveredImageIndex, (newValue) => {
  if (newValue !== null) {
    hoveredInfo.value.type = images[newValue].type;
    hoveredInfo.value.description = images[newValue].description;
  } else {
    hoveredInfo.value.type = '';
    hoveredInfo.value.description = '';
  }
});
</script>

<style scoped>
@import '../style/about.css';
</style>
