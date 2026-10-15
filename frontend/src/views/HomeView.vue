<template>
  <main class="home-page">
    <section class="home-hero">
      <div
        v-for="(slide, index) in slides"
        :key="slide.title"
        class="hero-slide"
        :class="{ active: slideIndex === index }"
        :style="{ backgroundImage: `linear-gradient(90deg, rgba(17, 35, 25, .76), rgba(17, 35, 25, .2)), url('${slide.image}')` }"
        aria-hidden="true"
      ></div>
      <div class="hero-content">
        <p class="eyebrow hero-eyebrow"><span></span> ANGELES CITY · PAMPANGA</p>
        <h1>{{ slides[slideIndex].title }}<br /><em>{{ slides[slideIndex].accent }}</em></h1>
        <p class="hero-copy">A city of old-town stories, bold Kapampangan flavors, and new adventures around every corner.</p>
        <form class="search-bar home-search" role="search" @submit.prevent="searchPlaces">
          <span class="search-icon" aria-hidden="true">⌕</span>
          <input v-model="query" type="search" aria-label="Search places" placeholder="Where would you like to go?" />
          <button class="btn-search" type="submit">Search <span aria-hidden="true">↗</span></button>
        </form>
        <p class="hero-note"><span aria-hidden="true">⌖</span> Discover Angeles City &amp; the Clark Freeport Zone</p>
      </div>
      <div class="slide-controls" aria-label="Choose hero image">
        <button type="button" aria-label="Previous slide" @click="changeSlide(-1)">‹</button>
        <button
          v-for="(slide, index) in slides"
          :key="slide.title"
          class="slide-dot"
          :class="{ active: slideIndex === index }"
          type="button"
          :aria-label="`Show slide ${index + 1}`"
          :aria-pressed="slideIndex === index"
          @click="slideIndex = index"
        ></button>
        <button type="button" aria-label="Next slide" @click="changeSlide(1)">›</button>
      </div>
      <div class="hero-caption">{{ slides[slideIndex].caption }}</div>
    </section>

  </main>
</template>

<script setup>
import { onMounted, onUnmounted, ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const query = ref('');
const slideIndex = ref(0);
let slideTimer;

const slides = [
  {
    title: 'Your next story',
    accent: 'starts here.',
    caption: 'OLD TOWN CHARM · ANGELES CITY',
    image: 'https://images.trvl-media.com/place/6129070/f18169f8-26a8-4f0b-a019-ea860a0c9edd.jpg'
  },
  {
    title: 'A taste of',
    accent: 'Kapampangan.',
    caption: 'LOCAL FLAVORS · PAMPANGA',
    image: 'https://sogbu.ph/wp-content/uploads/2026/05/kapampangan-cuisine-filipino-family-feast-pampanga-food-1024x576.png'
  },
  {
    title: 'Take the',
    accent: 'scenic route.',
    caption: 'A LITTLE ADVENTURE · CLARK & BEYOND',
    image: 'https://www.visitcentralluzon.com/wp-content/uploads/2025/09/Clark-Rolling-Tour-Parade-quarters-Pampanga-2021-Ripple-International-PR-scaled.jpg'
  }
];

function changeSlide(direction) {
  slideIndex.value = (slideIndex.value + direction + slides.length) % slides.length;
}

function searchPlaces() {
  router.push({ path: '/discover', query: query.value.trim() ? { q: query.value.trim() } : {} });
}

onMounted(() => {
  slideTimer = window.setInterval(() => changeSlide(1), 6000);
});
onUnmounted(() => window.clearInterval(slideTimer));
</script>
