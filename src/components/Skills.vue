<script setup>
import { computed, ref } from 'vue'

const activeCategory = ref('all')

const categories = [
  { id: 'all', label: 'All' },
  { id: 'frontend', label: 'Front-end' },
  { id: 'backend', label: 'Back-end' },
  { id: 'mobile', label: 'Mobile' },
  { id: 'tools', label: 'Tools' },
]

const skills = [
  // Front-end
  { name: 'React / Next.js', level: 'Advanced', category: 'frontend', tags: ['SPA', 'SSR', 'CSR'] },
  { name: 'Vue', level: 'Intermediate', category: 'frontend', tags: ['Composition API', 'Animations'] },
  { name: 'TypeScript', level: 'Advanced', category: 'frontend', tags: ['Typesafety', 'DX'] },
  { name: 'JavaScript', level: 'Advanced', category: 'frontend', tags: ['ES6+', 'DOM', 'Events'] },
  { name: 'TailwindCSS', level: 'Advanced', category: 'frontend', tags: ['Design system', 'Utility-first'] },
  { name: 'Styled-components', level: 'Advanced', category: 'frontend', tags: ['Theming', 'CSS-in-JS'] },
  { name: 'SCSS', level: 'Advanced', category: 'frontend', tags: ['Animation', 'Responsive'] },
  // Back-end
  { name: 'Node.js', level: 'Advanced', category: 'backend', tags: ['REST', 'Workers'] },
  { name: 'NestJS', level: 'Advanced', category: 'backend', tags: ['Clean architecture', 'DI'] },
  { name: 'Express', level: 'Intermediate', category: 'backend', tags: ['REST', 'Middleware'] },
  { name: 'PostgreSQL', level: 'Intermediate', category: 'backend', tags: ['SQL', 'Migrations'] },
  { name: 'MongoDB', level: 'Intermediate', category: 'backend', tags: ['NoSQL', 'Aggregation'] },
  //mobile 
  { name: 'React Native', level: 'Intermediate', category: 'mobile', tags: ['React Native', 'Expo', 'Nativewind'] },
  // Tools
  { name: 'Git / GitHub', level: 'Advanced', category: 'tools', tags: ['PR flow', 'CI basics'] },
  { name: 'Figma', level: 'Intermediate', category: 'tools', tags: ['Wireframes', 'Handoff'] },
]

const filteredSkills = computed(() => {
  if (activeCategory.value === 'all') return skills
  return skills.filter(skill => skill.category === activeCategory.value)
})
</script>

<template>
  <section id="skills" class="skills-section">
    <div class="section-header">
      <div>
        <p class="eyebrow">Skills</p>
        <h2>Stack & capabilities</h2>
        <p class="subtitle">
          Solid front-end focus with practical back-end experience and delivery tools.
        </p>
      </div>
      <div class="controls">
        <div class="pill-group">
          <button
            v-for="cat in categories"
            :key="cat.id"
            class="pill"
            :class="{ active: activeCategory === cat.id }"
            @click="activeCategory = cat.id"
          >
            {{ cat.label }}
          </button>
        </div>
      </div>
    </div>

    <div class="skill-grid">
      <article v-for="skill in filteredSkills" :key="skill.name" class="card">
        <div class="card-top">
          <div class="dot"></div>
          <span class="level">{{ skill.level }}</span>
        </div>
        <h3>{{ skill.name }}</h3>
        <div class="tags">
          <span v-for="tag in skill.tags" :key="tag" class="tag">{{ tag }}</span>
        </div>
      </article>
    </div>
  </section>
</template>

<style scoped>
.skills-section {
  padding: 80px 2rem;
}

.section-header {
  max-width: 1400px;
  margin: 0 auto 32px;
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 1.5rem;
  flex-wrap: wrap;
  position: relative;
  z-index: 2;
}

.eyebrow {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 6px 12px;
  border-radius: 999px;
  background: rgba(109, 75, 255, 0.15);
  color: #c3b5ff;
  font-weight: 600;
}

h2 {
  margin: 12px 0 8px;
  font-size: 2.6rem;
  line-height: 1.1;
}

.subtitle {
  max-width: 700px;
  color: #c6c9d4;
  margin: 0;
  line-height: 1.5;
}

.controls {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.pill-group {
  display: inline-flex;
  gap: 8px;
  padding: 6px;
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
  box-shadow: 0 8px 20px rgba(109, 75, 255, 0.3);
}

.skill-grid {
  max-width: 1400px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
  gap: 16px;
}

.card {
  background: rgba(0, 0, 0, 0.5);
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 16px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  transition: transform 0.2s ease, border-color 0.2s ease, box-shadow 0.2s ease;
  position: relative;
  z-index: 2;
}

.card:hover {
  transform: translateY(-3px);
  border-color: rgba(109, 75, 255, 0.5);
  box-shadow: 0 12px 26px rgba(0, 0, 0, 0.35);
}

.card-top {
  display: flex;
  align-items: center;
  gap: 8px;
}

.dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: linear-gradient(135deg, #6d4bff, #8a7bff);
  box-shadow: 0 0 12px rgba(109, 75, 255, 0.9);
}

.level {
  font-size: 0.85rem;
  color: #cfd4ff;
  padding: 4px 8px;
  border-radius: 8px;
  background: rgba(255, 255, 255, 0.06);
}

h3 {
  margin: 0;
  font-size: 1.1rem;
}

.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.tag {
  display: inline-flex;
  padding: 6px 10px;
  border-radius: 999px;
  background: rgba(109, 75, 255, 0.15);
  border: 1px solid rgba(109, 75, 255, 0.25);
  color: #e2ddff;
  font-size: 0.85rem;
  font-weight: 600;
}

@media (max-width: 768px) {
  .skills-section {
    padding: 60px 1.25rem;
  }

  h2 {
    font-size: 2.1rem;
  }

  .controls {
    width: 100%;
    justify-content: flex-start;
  }
}

@media (max-width: 520px) {
  .pill-group {
    flex-wrap: wrap;
    width: 100%;
  }
}
</style>

