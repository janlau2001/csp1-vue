<script setup>
import projects from '../data/projects.json'

function imageUrl(image) {
  return `${import.meta.env.BASE_URL}images/${image}`
}
</script>

<template>
  <section id="projects" class="py-5">
    <div class="container text-center my-4">
      <span class="badge-pill-nav">MY PROJECTS</span>
    </div>

    <div class="container pb-5">
      <div v-for="project in projects.featured" :key="project.id" class="portfolio-card mb-5">
        <div class="row align-items-center g-4">
          <div class="col-lg-5" :class="{ 'order-lg-2': project.imageOnRight }">
            <div class="profile-image-container">
              <img :src="imageUrl(project.image)" :alt="project.title" class="project-img">
            </div>
          </div>
          <div class="col-lg-7" :class="project.imageOnRight ? 'pe-lg-4' : 'ps-lg-4'">
            <span class="subtitle-text">{{ project.subtitle }}</span>
            <h2 class="project-title">{{ project.title }}</h2>
            <div class="inner-description-card mb-4">
              <p class="description-lead mb-4">{{ project.description }}</p>
              <div class="row g-3 project-metrics">
                <div v-for="metric in project.metrics" :key="metric.label" class="col-4">
                  <div class="metric-value">{{ metric.value }}</div>
                  <div class="metric-label">{{ metric.label }}</div>
                </div>
              </div>
            </div>
            <div class="d-flex align-items-center">
              <a :href="project.url" class="btn btn-white-pill" target="_blank" rel="noopener noreferrer">LIVE PLATFORM &nearr;</a>
            </div>
          </div>
        </div>
      </div>

      <div class="portfolio-card">
        <h3 class="h4 fw-bold mb-4 text-center">Upcoming &amp; Course Projects</h3>
        <div class="row g-4">
          <div v-for="project in projects.upcoming" :key="project.id" class="col-md-6 col-lg-4">
            <div class="inner-description-card h-100 d-flex flex-column justify-content-between">
              <div>
                <div class="project-thumb-container mb-3">
                  <img :src="imageUrl(project.image)" :alt="project.imageAlt" class="upcoming-card-img">
                </div>
                <span class="subtitle-text">{{ project.subtitle }}</span>
                <h4 class="upcoming-project">{{ project.title }}</h4>
                <p class="description-sub mt-2">{{ project.description }}</p>
              </div>
              <a href="#landing" class="btn btn-outline-pill mt-3 align-self-start">COMING SOON</a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
