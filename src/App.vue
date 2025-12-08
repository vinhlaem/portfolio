<script setup>
import Header from './components/Header.vue'
import Banner from './components/Banner.vue'
import SpaceBackground from './components/SpaceBackground.vue'
import About from './components/About.vue'
import Portfolio from './components/Portfolio.vue'
import Skills from './components/Skills.vue'
import Contact from './components/Contact.vue'
</script>

<template>
  <div class="app">
    <!-- Space Background with Multi-layer Parallax Stars -->
    <SpaceBackground 
      :star-count="[60, 50, 40]"
      :star-speed="[0.8, 0.5, 0.3]"
      :star-size="[2.0, 1.5, 1.2]"
      :glow-intensity="0.4"
      :blur-amount="100"
      :enable-twinkling="true"
    />
    
    <Header />
    <Banner />
    
    <!-- Sections for navigation -->
    <About/>

    <Portfolio/>

    <Skills/>

    <Contact/>
  </div>
</template>

<style>
/* Custom Scrollbar Styles */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

/* Webkit browsers (Chrome, Safari, Edge) */
::-webkit-scrollbar {
  width: 10px;
}

::-webkit-scrollbar-track {
  background: #000000;
}

::-webkit-scrollbar-thumb {
  background: linear-gradient(180deg, #1a1a1a 0%, #0a0a0a 100%);
  border-radius: 5px;
  border: 2px solid #000000;
}

::-webkit-scrollbar-thumb:hover {
  background: linear-gradient(180deg, #2a2a2a 0%, #1a1a1a 100%);
}

/* Firefox */
* {
  scrollbar-width: thin;
  scrollbar-color: #1a1a1a #000000;
}

/* App Styles */
.app {
  min-height: 100vh;
  background: #000000;
  color: white;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  position: relative;
}




.section-content {
  max-width: 1400px;
  width: 100%;
  margin: 0 auto;
  text-align: center;
}

.section-content h2 {
  font-size: 3rem;
  margin-bottom: 2rem;
  color: white;
}

.section-content p {
  font-size: 1.25rem;
  opacity: 0.8;
}

/* Responsive */
@media (max-width: 768px) {
  .section {
    padding: 100px 1.5rem 3rem;
  }

  .section-content h2 {
    font-size: 2rem;
  }
}
</style>