<script setup>
import { onMounted, onUnmounted, ref } from 'vue'

const visible = ref(false)

function updateVisibility() {
  visible.value = window.scrollY > 300
}

function scrollToTop() {
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

onMounted(() => {
  updateVisibility()
  window.addEventListener('scroll', updateVisibility, { passive: true })
})
onUnmounted(() => window.removeEventListener('scroll', updateVisibility))
</script>

<template>
  <button id="scrollTopBtn" :class="{ show: visible }" title="Go to top" aria-label="Scroll to top" @click="scrollToTop">
    <i class="bi bi-arrow-up"></i>
  </button>
</template>
