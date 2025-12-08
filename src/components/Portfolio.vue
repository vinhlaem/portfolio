<script setup>
import { computed, ref } from 'vue'

const viewMode = ref('grid') // 'grid' | 'row'
const activeCategory = ref('all')

const openProject = (url) => {
  if(url){
    window.open(url, '_blank')
  }
}

const projects = [
  {
    title: 'Wedding',
    summary: 'This is a wedding website for me',
    badges: ['Online', 'VueJS', "NodeJS", 'Typescript', 'JavaScript', 'MongoDB'],
    category: 'web',
    image: '/projects/wedding.png',
    url: 'https://wedding-rose-iota.vercel.app/',
  },
  {
    title: 'Crewn',
    summary: 'The platform is an e-commerce site specializing in MagSafe accessories, featuring product search, rich category browsing, and order tracking.',
    badges: ['Online', 'Next.js',  'Typescript', 'Fabric', 'Bootstrap', 'SCSS', 'NodeJs', 'Postgresql'],
    category: 'web',
    image: '/projects/crewn.png',
    url: 'https://crewn.vercel.app/',
  },
  {
    title: 'DexFM - Mobile App',
    summary: ' Build a mobile app that integrates information about coin prices and user can trading in this app, utilizing the same API as the web version.',
    badges: ['Online',  'React native', 'API Rest', 'NestJs', 'Postgresql', 'TypeScript'],
    category: 'app',
    image: '/projects/dex-app.png',
  },
  {
    title: 'Meme warrior space',
    summary: 'Build a website for users to create their own memes and push them to the spaces they have joined to participate in the events of that space, and also mint the memes in that space',
    badges: ['Online', 'Next.js', 'Typescript', 'React Query', 'Rainbownkit', 'Wagmi', 'FabricJs'],
    category: 'web',
    image: '/projects/meme.ws.png',
    url: "https://meme.ws/"
  },
  {
    title: 'DexFM - Web',
    summary: 'Build a website that integrates information about coin prices and user can trading in this website',
    badges: ['NextJs', 'Rainbownkit', 'Wagmi', 'NestJs', 'Postgresql', 'TypeScript', 'React Query',],
    category: 'web',
    image: '/projects/dex.web.png',
    url: 'https://dex.fm/',
  },
  {
    title: 'Coin+',
    summary: ' Build a website that integrates information about coin prices',
    badges: ['NextJs', 'SCSS', 'NestJs', 'Postgresql'],
    category: 'web',
    image: '/projects/coins.png',
    url: 'https://coins.plus/',
  },
  {
    title: 'Trainsay - Telegram App',
    summary: 'Trainsay is a Telegram app where users record their voice based on provided text. Admin scores the recordings, and during the airdrop, the scores are converted into project tokens.',
    badges: ['ReactJs', 'NodeJs', 'MongoDB','TypeScript', 'Scss', 'JavaScript'],
    category: 'web',
  },
  {
    title: 'POPOY',
    summary: 'Build an introduction website for the POPOY coin development project',
    badges: ['NextJs', 'Connectkit', 'Wagmi'],
    category: 'web',
  },

  
]

const filteredProjects = computed(() => {
  if (activeCategory.value === 'all') return projects
  return projects.filter(p => p.category === activeCategory.value)
})
</script>

<template>
  <section id="portfolio" class="section">
    <div class="section-header">
      <div>
        <p class="eyebrow">Portfolio</p>
        <h2>Works and projects</h2>
      </div>
      <div class="controls">
      
        <div class="view-toggle">
          <button
            class="pill"
            :class="{ active: viewMode === 'grid' }"
            @click="viewMode = 'grid'"
            aria-label="Grid view"
          >
            Grid
          </button>
          <button
            class="pill"
            :class="{ active: viewMode === 'row' }"
            @click="viewMode = 'row'"
            aria-label="Row view"
          >
            Row
          </button>
        </div>
      </div>
    </div>

    <div :class="['projects', viewMode]">
      <article
        v-for="project in filteredProjects"
        :key="project.title"
        class="card"
        @click="openProject(project.url)"
      >
        <div class="card-media" :style="{ backgroundImage: project.image ? `url(${project.image})` : '' }">
          <div v-if="!project.image" class="media-fallback">{{ project.title }}</div>
        </div>
        <div class="card-body">
          <h3>{{ project.title }}</h3>
          <p class="summary">{{ project.summary }}</p>
          <div class="badges">
            <span v-for="badge in project.badges" :key="badge" class="badge">
              {{ badge }}
            </span>
          </div>
        </div>
      </article>
    </div>
  </section>
</template>

<style scoped>
.section {
  min-height: 100vh;
  padding: 80px 2rem;
}

.section-header {
  max-width: 1400px;
  margin: 0 auto 32px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1.5rem;
  flex-wrap: wrap;
  position: relative;
  z-index: 9999999;
}

.eyebrow {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 6px 12px;
  border-radius: 999px;
  background: rgba(120, 87, 255, 0.12);
  color: #c3b5ff;
  font-size: 0.95rem;
  font-weight: 600;
}

h2 {
  margin-top: 12px;
  font-size: 2.8rem;
  line-height: 1.1;
}

.controls {
  display: flex;
  align-items: center;
  gap: 1rem;
  flex-wrap: wrap;
}

.category-group,
.view-toggle {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 4px;
  border-radius: 14px;
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.05);
}

.pill {
  border: none;
  background: transparent;
  color: #cfd4ff;
  padding: 8px 14px;
  border-radius: 10px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
}

.pill.active {
  background: linear-gradient(135deg, #6d4bff, #8a7bff);
  color: #fff;
  box-shadow: 0 8px 20px rgba(109, 75, 255, 0.35);
}

.projects.grid {
  max-width: 1400px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 20px;
}

.projects.row {
  max-width: 1400px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.card {
  background: rgba(0,0,0, 0.5);
  position: relative;
  z-index: 9999999;
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 18px;
  overflow: hidden;
  display: flex;
  flex-direction: column;

  gap: 12px;
  transition: transform 0.2s ease, border-color 0.2s ease, box-shadow 0.2s ease;
}

.projects.row .card {
  flex-direction: row;
  align-items: stretch;
}

.card:hover {
  transform: translateY(-4px);
  border-color: rgba(109, 75, 255, 0.5);
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.35);
}

.card-media {
  width: 100%;
  min-height: 180px;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  position: relative;
}

.projects.row .card-media {
  width: 320px;
  min-height: 200px;
  flex-shrink: 0;
}

.media-fallback {
  width: 100%;
  height: 100%;
  display: grid;
  place-items: center;
  background: radial-gradient(circle at 20% 20%, rgba(109, 75, 255, 0.35), transparent 35%),
              radial-gradient(circle at 80% 30%, rgba(255, 255, 255, 0.15), transparent 40%),
              #12141c;
  color: #fff;
  font-weight: 700;
}

.card-body {
  padding: 10px 18px 18px;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.card-body h3 {
  margin: 0;
  font-size: 1.2rem;
}

.summary {
  margin: 0;
  color: #c6c9d4;
  line-height: 1.5;
  min-height: 60px;
}

.badges {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.badge {
  display: inline-flex;
  padding: 6px 10px;
  border-radius: 999px;
  background: rgba(109, 75, 255, 0.18);
  border: 1px solid rgba(109, 75, 255, 0.25);
  color: #e2ddff;
  font-size: 0.85rem;
  font-weight: 600;
}

@media (max-width: 960px) {
  h2 {
    font-size: 2.1rem;
  }

  .projects.row .card {
    flex-direction: column;
  }

  .projects.row .card-media {
    width: 100%;
    min-height: 180px;
  }
}

@media (max-width: 600px) {
  .section {
    padding: 60px 1rem;
  }

  .category-group,
  .view-toggle {
    width: 100%;
    justify-content: space-between;
  }
}
</style>