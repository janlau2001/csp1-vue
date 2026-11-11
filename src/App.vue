<script setup>
import Navbar from './components/Navbar.vue'
import LandingSection from './components/LandingSection.vue'
import ProjectsSection from './components/ProjectsSection.vue'
import ToolsSection from './components/ToolsSection.vue'
import ContactSection from './components/ContactSection.vue'
import ScrollToTop from './components/ScrollToTop.vue'
import SiteFooter from './components/SiteFooter.vue'
</script>

<template>
  <Navbar />
    <main>
      <LandingSection />
      <ProjectsSection />
      <ToolsSection />
      <ContactSection />
    </main>
  <ScrollToTop />
  <SiteFooter />
</template>
