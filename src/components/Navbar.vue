<script setup>
import { ref } from 'vue'

const isOpen = ref(false)
</script>

<template>
  <nav class="navbar navbar-expand-lg navbar-dark py-3">
    <div class="container">
      <a class="navbar-brand d-flex align-items-center gap-2" href="#landing" @click="isOpen = false">
        <span class="logo-pill">OLARTE PORTFOLIO</span>
      </a>
      <button class="navbar-toggler" type="button" aria-controls="navbarNav"
        :aria-expanded="isOpen" aria-label="Toggle navigation" @click="isOpen = !isOpen">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div id="navbarNav" class="collapse navbar-collapse" :class="{ show: isOpen }">
        <ul class="navbar-nav ms-auto align-items-center gap-2">
          <li class="nav-item"><a class="nav-link custom-nav-link" href="#landing" @click="isOpen = false">HOME</a></li>
          <li class="nav-item"><a class="nav-link custom-nav-link" href="#projects" @click="isOpen = false">PROJECTS</a></li>
          <li class="nav-item"><a class="nav-link custom-nav-link" href="#tools" @click="isOpen = false">TOOLS</a></li>
          <li class="nav-item"><a class="nav-link custom-nav-link" href="#contact" @click="isOpen = false">CONTACTS</a></li>
        </ul>
      </div>
    </div>
  </nav>
</template>
