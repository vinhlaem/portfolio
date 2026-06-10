<script setup>
import { computed } from 'vue'

const CAREER_START_YEAR = 2022
const CAREER_START_MONTH = 5 // June, zero-based month index

const experienceYears = computed(() => {
  const now = new Date()
  let years = now.getFullYear() - CAREER_START_YEAR

  if (now.getMonth() < CAREER_START_MONTH) {
    years -= 1
  }

  return Math.max(years, 0)
})

const experienceYearsLabel = computed(() => `${experienceYears.value}+ years`)

const experiences = computed(() => [
  {
    title: 'IT Graduate',
    experience: 'Bachelor of Information Technology <br/> <b>Class of 2023</b>',
    icon: '/icons/graduated.svg',
  },
  {
    title: 'Full-Stack Developer',
    experience: `<b>${experienceYearsLabel.value}</b> of professional experience <br/> building web applications`,
    icon: '/icons/developer.svg',
  },
  {
    title: 'Tech Stack',
    experience: '<b>React, Next.js, Node.js, NestJS</b> <br/> PostgreSQL, MongoDB, TypeScript',
    icon: '/icons/fullstack.svg',
  },
])
</script>

<template>
    <section id="about" class="section">
        <div class="section-content">
            <h2>About Me</h2>
        </div>
        <div class="experience">
            <div class="experience-item" v-for="(item, index) in experiences" :key="item.title"
                :style="{ animationDelay: `${index * 0.2}s` }">
                <!-- Purple glowing halo -->
                <div class="item-halo"></div>

                <!-- Content -->
                <div class="item-content">
                    <img :src="item.icon" alt="icon" class="experience-icon" loading="lazy" decoding="async">
                    <p v-html="item.experience"></p>
                </div>
            </div>
        </div>

        <div class="about-content">
            <div class="about-container">
                <!-- Profile Picture on Left -->
                <div class="profile-picture-wrapper">
                    <div class="profile-halo"></div>
                    <div class="profile-image-container">
                        <img
                            src="/optimized/avatar-480.jpg"
                            srcset="/optimized/avatar-320.jpg 320w, /optimized/avatar-480.jpg 480w, /optimized/avatar-754.jpg 754w"
                            sizes="(max-width: 480px) 250px, (max-width: 768px) 280px, (max-width: 1024px) 350px, 400px"
                            alt="Profile"
                            class="profile-image"
                            width="754"
                            height="552"
                            loading="lazy"
                            decoding="async"
                        >
                    </div>
                </div>

                <!-- Content on Right -->
                <div class="about-text-content">
                    <!-- About me button -->
                    <button class="about-button">
                        <span class="button-emoji">💡</span>
                        <span>About me</span>
                    </button>

                    <!-- Name -->
                    <h1 class="about-name">
                        Truong Dinh Vinh
                    </h1>

                    <!-- Professional Introduction -->
                    <p class="about-description">
                        I'm a Front-End Developer with {{ experienceYearsLabel }} of experience building responsive, user-friendly, and
                        performance-optimized web applications. I work primarily with modern front-end technologies and
                        have hands-on knowledge of backend development, including the ability to design and implement
                        CRUD APIs when needed. I'm looking for opportunities in a dynamic and professional environment
                        where I can contribute my expertise, collaborate with experienced engineers, and continue
                        improving my technical skills, problem-solving abilities, and overall product thinking.
                    </p>

                    <!-- Main Interests -->
                    <div class="about-section">
                        <div class="section-header">
                            <span class="section-title">Main interests: <span class="description-content">
                                    Front-end Development (React.js, Next.js, Vue.js, TypeScript), UI/UX Design
                                    fundamentals, and Full-stack Development (Node.js, NestJS, Express, MySQL, MongoDB)
                                </span>
                            </span>
                        </div>

                    </div>

                    <!-- Goal -->
                    <div class="about-section">
                        <div class="section-header">
                            <span class="section-title">Goal: <span class="description-content">
                                    To create digital products that have a positive impact on people's lives, constantly
                                    improve
                                    my skills, and learn new technologies.
                                </span>
                            </span>
                        </div>

                    </div>
                </div>
            </div>
        </div>
    </section>
</template>


<style>
.section {
    min-height: 100vh;
    padding: 50px 0px;
}

.section-content {
    text-align: center;
    margin-bottom: 3rem;
    position: relative;
    z-index: 2;

}

.section-content h2 {
    font-size: 3rem;
    font-weight: 700;
    color: #ffffff;
    margin: 0;
    animation: fadeInUp 0.6s ease;
}

.experience {
    display: flex;
    position: relative;
    z-index: 2;

    gap: 3rem;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    margin-top: 100px;
    width: 100%;
}

.experience-item {
    min-width: 305px;
    position: relative;
    z-index: 2;
    animation: fadeInUp 0.8s ease both;
}

.item-halo {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 250px;
    height: 250px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(139, 92, 246, 0.6) 0%, rgba(139, 92, 246, 0.4) 40%, rgba(139, 92, 246, 0.1) 70%, transparent 100%);
    filter: blur(40px);
    animation: pulse 3s ease-in-out infinite;
    z-index: 2;
    pointer-events: none;
}

@keyframes pulse {

    0%,
    100% {
        transform: translate(-50%, -50%) scale(1);
        opacity: 0.8;
    }

    50% {
        transform: translate(-50%, -50%) scale(1.1);
        opacity: 1;
    }
}

.item-content {
    position: relative;
    z-index: 2;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;
    min-width: 305px;
    width: 100%;
    border: 1px solid rgba(139, 92, 246, 0.3);
    border-radius: 1rem;
    padding: 1.5rem;
    background: #040404cf;
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    box-shadow: 0 10px 30px rgba(139, 92, 246, 0.2);
    transition: all 0.3s ease;
}

.experience-item:hover .item-content {
    box-shadow: 0 15px 40px rgba(139, 92, 246, 0.3);
    border-color: rgba(139, 92, 246, 0.5);
}

.experience-icon {
    width: 60px;
    height: 60px;
    filter: drop-shadow(0 5px 15px rgba(139, 92, 246, 0.4));
    transition: transform 0.3s ease;
}

.experience-item:hover .experience-icon {
    transform: scale(1.1) rotate(5deg);
}

.experience-item h3 {
    font-size: 1.5rem;
    font-weight: 600;
    color: #ffffff;
    margin: 0;
}

.experience-item p {
    text-align: center;
    font-size: 1.5rem;
    font-weight: 500;
    color: rgba(255, 255, 255, 0.9);
    margin: 0;
    line-height: 1.6;
}

.experience-item p b {
    color: rgba(139, 92, 246, 1);
    font-weight: 700;
    font-size: 1.7rem;
}

@keyframes fadeInUp {
    from {
        opacity: 0;
        transform: translateY(30px);
    }

    to {
        opacity: 1;
        transform: translateY(0);
    }
}

/* Responsive */
@media (max-width: 768px) {
    .section {
        padding: 50px 16px;
    }

    .section-content h2 {
        font-size: 2rem;
    }

    .experience {
        gap: 2rem;
        margin-top: 50px;
    }

    .experience-item {
        min-width: 100%;
        max-width: 300px;
    }

    .item-halo {
        width: 180px;
        height: 180px;
    }

    .item-content {
        padding: 1.25rem;
    }

    .experience-icon {
        width: 50px;
        height: 50px;
    }

    .experience-item p {
        font-size: 1rem;
    }
}

@media (max-width: 480px) {
    .section-content h2 {
        font-size: 1.75rem;
    }

    .experience {
        gap: 1.5rem;
    }

    .item-content {
        padding: 1rem;
    }
}

/* About Content Section */
.about-content {
    margin-top: 6rem;
    max-width: 1400px;
    margin-left: auto;
    margin-right: auto;
    padding: 0 2rem;
    position: relative;
    z-index: 2;
}

.about-container {
    display: flex;
    gap: 4rem;
    align-items: flex-start;
    position: relative;
}

/* Profile Picture Section */
.profile-picture-wrapper {
    position: relative;
    flex-shrink: 0;
    width: 400px;
    height: 400px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.profile-halo {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 450px;
    height: 450px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(139, 92, 246, 0.6) 0%, rgba(139, 92, 246, 0.4) 40%, rgba(139, 92, 246, 0.1) 70%, transparent 100%);
    filter: blur(40px);
    animation: pulse 3s ease-in-out infinite;
    z-index: 0;
    pointer-events: none;
}

.profile-image-container {
    position: relative;
    z-index: 2;
    width: 400px;
    height: 400px;
    border-radius: 50%;
    overflow: hidden;
    background: transparent;
    box-shadow: 0 20px 60px rgba(139, 92, 246, 0.3);
}

.profile-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center;
}

/* About Text Content */
.about-text-content {
    flex: 1;
    color: white;
    display: flex;
    flex-direction: column;
    gap: 2rem;
    animation: fadeInUp 0.8s ease 0.2s both;
}

.about-button {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.5rem 1rem;
    background: rgba(139, 92, 246, 0.9);
    border: none;
    border-radius: 0.5rem;
    color: white;
    font-size: 0.9rem;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.3s ease;
    width: fit-content;
}

.about-button:hover {
    background: rgba(139, 92, 246, 1);
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(139, 92, 246, 0.4);
}

.button-emoji {
    font-size: 1.2rem;
}

.about-name {
    font-size: 3.5rem;
    font-weight: 700;
    color: white;
    margin: 0;
    line-height: 1.2;
    animation: fadeInUp 0.8s ease 0.4s both;
}

.about-description {
    font-size: 1.1rem;
    line-height: 1.8;
    color: rgba(255, 255, 255, 0.9);
    margin: 0;
    animation: fadeInUp 0.8s ease 0.6s both;
}

.description-content {
    font-size: 1.1rem;
    font-weight: 400;
}

.about-section {
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
    animation: fadeInUp 0.8s ease both;
}

.about-section:nth-of-type(1) {
    animation-delay: 0.8s;
}

.about-section:nth-of-type(2) {
    animation-delay: 1s;
}

.section-header {
    display: flex;
    align-items: center;
    gap: 0.5rem;
}

.section-emoji {
    font-size: 1.5rem;
}

.section-title {
    font-size: 1.2rem;
    font-weight: 600;
    color: white;
}

.section-content {
    font-size: 1rem;
    line-height: 1.7;
    color: rgba(255, 255, 255, 0.85);
    margin: 0;
    padding-left: 2rem;
}

/* Responsive for About Content */
@media (max-width: 1024px) {
    .about-container {
        flex-direction: column;
        align-items: center;
        gap: 3rem;
    }

    .profile-picture-wrapper {
        width: 350px;
        height: 350px;
    }

    .profile-halo {
        width: 400px;
        height: 400px;
    }

    .profile-image-container {
        width: 350px;
        height: 350px;
    }

    .about-text-content {
        text-align: center;
        align-items: center;
    }

    .section-content {
        padding-left: 0;
        text-align: left;
    }
}

@media (max-width: 768px) {
    .about-content {
        margin-top: 4rem;
        padding: 0 1rem;
    }

    .profile-picture-wrapper {
        width: 280px;
        height: 280px;
    }

    .profile-halo {
        width: 320px;
        height: 320px;
    }

    .profile-image-container {
        width: 280px;
        height: 280px;
    }

    .about-name {
        font-size: 2.5rem;
    }

    .about-description {
        font-size: 1rem;
    }

    .section-title {
        font-size: 1.1rem;
    }

    .section-content {
        font-size: 0.95rem;
    }
}

@media (max-width: 480px) {
    .profile-picture-wrapper {
        width: 250px;
        height: 250px;
    }

    .profile-halo {
        width: 280px;
        height: 280px;
    }

    .profile-image-container {
        width: 250px;
        height: 250px;
    }

    .about-name {
        font-size: 2rem;
    }
}

@media (prefers-reduced-motion: reduce) {
    .section-content h2,
    .experience-item,
    .item-halo,
    .profile-halo,
    .about-text-content,
    .about-name,
    .about-description,
    .about-section {
        animation: none;
    }

    .item-content,
    .experience-icon,
    .about-button {
        transition: none;
    }
}
</style>
