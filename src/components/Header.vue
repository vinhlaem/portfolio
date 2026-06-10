<template>
  <header class="header" :class="{ scrolled: isScrolled }">
    <div class="header-container">
      <!-- Logo -->
      <div class="logo-wrapper">
        <img
          src="/optimized/logo-160.png"
          srcset="/optimized/logo-96.png 96w, /optimized/logo-160.png 160w"
          sizes="60px"
          alt="Logo"
          class="logo"
          width="160"
          height="160"
          decoding="async"
          fetchpriority="low"
        />
      </div>

      <!-- Desktop Navigation -->
      <nav class="nav-desktop">
        <a
          v-for="link in navLinks"
          :key="link.id"
          :href="`#${link.id}`"
          @click.prevent="scrollToSection(link.id)"
          class="nav-link"
          :class="{ active: activeSection === link.id }"
        >
          {{ link.label }}
        </a>
      </nav>

      <!-- Mobile Menu Button -->
      <button
        class="menu-toggle"
        @click="toggleDrawer"
        aria-label="Toggle menu"
      >
        <span class="hamburger" :class="{ active: drawerOpen }">
          <span></span>
          <span></span>
          <span></span>
        </span>
      </button>
    </div>

    <!-- Mobile Drawer Sidebar -->
    <div
      class="drawer-overlay"
      :class="{ active: drawerOpen }"
      @click="closeDrawer"
    ></div>
    <aside class="drawer" :class="{ active: drawerOpen }">
      <div class="drawer-header">
        <img
          src="/optimized/logo-96.png"
          alt="Logo"
          class="drawer-logo"
          width="96"
          height="96"
          loading="lazy"
          decoding="async"
        />
        <button
          class="drawer-close"
          @click="closeDrawer"
          aria-label="Close menu"
        >
          ✕
        </button>
      </div>
      <nav class="drawer-nav">
        <a
          v-for="link in navLinks"
          :key="link.id"
          :href="`#${link.id}`"
          @click.prevent="handleDrawerLinkClick(link.id)"
          class="drawer-link"
          :class="{ active: activeSection === link.id }"
        >
          {{ link.label }}
        </a>
      </nav>
    </aside>
  </header>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const drawerOpen = ref(false);
const activeSection = ref("home");
const isScrolled = ref(false);
let scrollFrame = null;
let sectionObserver = null;
let observerRefreshTimeout = null;
let mutationObserver = null;
let trackingTimer = null;
const observedSections = new Set();

const navLinks = [
  { id: "home", label: "Home" },
  { id: "about", label: "About me" },
  { id: "portfolio", label: "Portfolio" },
  { id: "skills", label: "Skills" },
  { id: "contact", label: "Contact" },
];

const scrollToSection = (sectionId) => {
  const element = document.getElementById(sectionId);
  if (element) {
    const offset = 80; // Header height offset
    const elementPosition = element.getBoundingClientRect().top;
    const offsetPosition = elementPosition + window.pageYOffset - offset;

    window.scrollTo({
      top: offsetPosition,
      behavior: "smooth",
    });
  }
};

const handleDrawerLinkClick = (sectionId) => {
  scrollToSection(sectionId);
  closeDrawer();
};

const toggleDrawer = () => {
  drawerOpen.value = !drawerOpen.value;
  document.body.style.overflow = drawerOpen.value ? "hidden" : "";
};

const closeDrawer = () => {
  drawerOpen.value = false;
  document.body.style.overflow = "";
};

const handleScroll = () => {
  if (scrollFrame) return;

  scrollFrame = window.requestAnimationFrame(() => {
    isScrolled.value = window.scrollY > 50;
    scrollFrame = null;
  });
};

const observeSections = () => {
  if (!sectionObserver) return;

  navLinks.forEach((link) => {
    const section = document.getElementById(link.id);
    if (section && !observedSections.has(link.id)) {
      sectionObserver.observe(section);
      observedSections.add(link.id);
    }
  });

  if (observedSections.size === navLinks.length) {
    mutationObserver?.disconnect();
    mutationObserver = null;
  }
};

const setupSectionTracking = () => {
  sectionObserver = new IntersectionObserver(
    (entries) => {
      const visibleEntry = entries
        .filter((entry) => entry.isIntersecting)
        .sort((a, b) => b.intersectionRatio - a.intersectionRatio)[0];

      if (visibleEntry?.target?.id) {
        activeSection.value = visibleEntry.target.id;
      }
    },
    {
      rootMargin: "-30% 0px -55% 0px",
      threshold: [0, 0.25, 0.5, 0.75],
    }
  );

  observeSections();
  observerRefreshTimeout = window.setTimeout(observeSections, 1500);
  mutationObserver = new MutationObserver(observeSections);
  const appRoot = document.getElementById("app");
  if (appRoot) {
    mutationObserver.observe(appRoot, {
      childList: true,
      subtree: true,
    });
  }

  window.addEventListener("scroll", handleScroll, { passive: true });
  handleScroll();
};

onMounted(() => {
  trackingTimer = window.setTimeout(setupSectionTracking, 5000);
});

onUnmounted(() => {
  if (trackingTimer) {
    window.clearTimeout(trackingTimer);
  }
  if (scrollFrame) {
    window.cancelAnimationFrame(scrollFrame);
  }
  if (observerRefreshTimeout) {
    window.clearTimeout(observerRefreshTimeout);
  }
  mutationObserver?.disconnect();
  sectionObserver?.disconnect();
  window.removeEventListener("scroll", handleScroll);
  document.body.style.overflow = "";
});
</script>

<style scoped>
.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background: transparent;
  backdrop-filter: none;
  z-index: 1000;
  box-shadow: none;
  border-bottom: 1px solid transparent;
  transition: background 0.3s ease, box-shadow 0.3s ease,
    border-bottom-color 0.3s ease;
}

.header.scrolled {
  background: rgba(0, 0, 0, 0.95);
  backdrop-filter: blur(10px);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.5);
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
}

.header-container {
  max-width: 1400px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem 2rem;
  position: relative;
}

.logo-wrapper {
  display: flex;
  align-items: center;
}

.logo {
  height: 60px;
  width: auto;
  object-fit: contain;
}

.nav-desktop {
  display: flex;
  align-items: center;
  gap: 2rem;
}

.nav-link {
  color: white;
  text-decoration: none;
  font-size: 0.95rem;
  font-weight: 500;
  transition: opacity 0.3s ease;
  position: relative;
  padding: 0.5rem 0;
}

.nav-link:hover {
  opacity: 0.8;
}

.nav-link.active {
  opacity: 1;
}

.nav-link.active::after {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: white;
}

.menu-toggle {
  display: none;
  background: transparent;
  border: none;
  cursor: pointer;
  padding: 0.5rem;
  z-index: 1001;
}

.hamburger {
  display: flex;
  flex-direction: column;
  gap: 4px;
  width: 24px;
}

.hamburger span {
  display: block;
  height: 2px;
  width: 100%;
  background: white;
  transition: all 0.3s ease;
  border-radius: 2px;
}

.hamburger.active span:nth-child(1) {
  transform: rotate(45deg) translate(5px, 5px);
}

.hamburger.active span:nth-child(2) {
  opacity: 0;
}

.hamburger.active span:nth-child(3) {
  transform: rotate(-45deg) translate(7px, -6px);
}

/* Drawer Styles */
.drawer-overlay {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 1001;
  opacity: 0;
  transition: opacity 0.3s ease;
  pointer-events: none;
}

.drawer-overlay.active {
  display: block;
  opacity: 1;
  pointer-events: all;
}

.drawer {
  position: fixed;
  top: 0;
  right: -100%;
  width: 280px;
  height: 100vh;
  background: rgba(0, 0, 0, 0.98);
  backdrop-filter: blur(10px);
  z-index: 1002;
  transition: right 0.3s ease;
  box-shadow: -2px 0 10px rgba(0, 0, 0, 0.8);
  border-left: 1px solid rgba(255, 255, 255, 0.05);
}

.drawer.active {
  right: 0;
}

.drawer-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1.5rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.drawer-logo {
  height: 28px;
  width: auto;
  object-fit: contain;
}

.drawer-close {
  background: transparent;
  border: none;
  color: white;
  font-size: 1.5rem;
  cursor: pointer;
  padding: 0.25rem;
  line-height: 1;
  transition: opacity 0.3s ease;
}

.drawer-close:hover {
  opacity: 0.7;
}

.drawer-nav {
  display: flex;
  flex-direction: column;
  padding: 1rem 0;
}

.drawer-link {
  color: white;
  text-decoration: none;
  font-size: 1rem;
  padding: 1rem 1.5rem;
  transition: background 0.3s ease;
  border-left: 3px solid transparent;
}

.drawer-link:hover {
  background: rgba(255, 255, 255, 0.1);
}

.drawer-link.active {
  background: rgba(255, 255, 255, 0.15);
  border-left-color: white;
}

/* Responsive */
@media (max-width: 768px) {
  .nav-desktop {
    display: none;
  }

  .menu-toggle {
    display: block;
  }

  .header-container {
    padding: 1rem 1.5rem;
  }
}

@media (max-width: 480px) {
  .header-container {
    padding: 0.75rem 1rem;
  }

  .logo {
    height: 28px;
  }

  .drawer {
    width: 100%;
    max-width: 320px;
  }
}
</style>
