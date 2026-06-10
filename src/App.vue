<script setup>
import {
  defineAsyncComponent,
  onMounted,
  onUnmounted,
  reactive,
  ref,
} from "vue";
import Header from "./components/Header.vue";
import Banner from "./components/Banner.vue";

const SpaceBackground = defineAsyncComponent(
  () => import("./components/SpaceBackground.vue"),
);
const About = defineAsyncComponent(() => import("./components/About.vue"));
const Portfolio = defineAsyncComponent(
  () => import("./components/Portfolio.vue"),
);
const Skills = defineAsyncComponent(() => import("./components/Skills.vue"));
const Contact = defineAsyncComponent(() => import("./components/Contact.vue"));

const lazySections = ["about", "portfolio", "skills", "contact"];
const loadedSections = reactive({
  about: false,
  portfolio: false,
  skills: false,
  contact: false,
});

let sectionObserver = null;
let backgroundTimer = null;
const showBackground = ref(false);

const loadSection = (id) => {
  if (id in loadedSections) {
    loadedSections[id] = true;
  }
};

onMounted(() => {
  backgroundTimer = window.setTimeout(() => {
    showBackground.value = true;
  }, 500);

  if (!("IntersectionObserver" in window)) {
    lazySections.forEach(loadSection);
    return;
  }

  sectionObserver = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (!entry.isIntersecting) return;

        const id = entry.target.id;
        loadSection(id);
        sectionObserver?.unobserve(entry.target);
      });
    },
    {
      rootMargin: "0px 0px",
      threshold: 0.01,
    },
  );

  lazySections.forEach((id) => {
    const section = document.getElementById(id);
    if (section) sectionObserver.observe(section);
  });
});

onUnmounted(() => {
  if (backgroundTimer) {
    window.clearTimeout(backgroundTimer);
  }
  sectionObserver?.disconnect();
});
</script>

<template>
  <div class="app">
    <!-- Space Background with Multi-layer Parallax Stars -->
    <SpaceBackground
      v-if="showBackground"
      :star-count="[18, 14]"
      :star-speed="[0.25, 0.14]"
      :star-size="[2.0, 1.5, 1.2]"
      :glow-intensity="0.18"
      :blur-amount="32"
      :enable-twinkling="true"
    />

    <Header />
    <Banner />

    <!-- Sections for navigation -->
    <About v-if="loadedSections.about" />
    <section
      v-else
      id="about"
      class="lazy-section-placeholder"
      aria-label="About section"
    ></section>

    <Portfolio v-if="loadedSections.portfolio" />
    <section
      v-else
      id="portfolio"
      class="lazy-section-placeholder"
      aria-label="Portfolio section"
    ></section>

    <Skills v-if="loadedSections.skills" />
    <section
      v-else
      id="skills"
      class="lazy-section-placeholder"
      aria-label="Skills section"
    ></section>

    <Contact v-if="loadedSections.contact" />
    <section
      v-else
      id="contact"
      class="lazy-section-placeholder"
      aria-label="Contact section"
    ></section>
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
  font-family:
    -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu,
    Cantarell, sans-serif;
  position: relative;
}

.lazy-section-placeholder {
  min-height: 100vh;
  position: relative;
  z-index: 1;
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
