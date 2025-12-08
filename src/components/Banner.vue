<template>
  <section id="home" class="banner">
    <div class="banner-container">
      <div class="banner-content">
        <h2 class="banner-greeting">👋 Greetings!</h2>
        <h1 class="banner-name">Vinh Truong</h1>
        <p class="banner-profession">FullStack developer</p>
        <div class="banner-description">
          <span class="typing-text">{{ displayedText }}</span>
          <span class="cursor" :class="{ blinking: showCursor }">|</span>
        </div>
      </div>

      <div class="banner-avatar">
        <!-- Purple glowing halo behind avatar -->
        <div class="avatar-halo"></div>
        
        <!-- Avatar image -->
        <div class="avatar-image-wrapper">
          <img src="/avatar.png" alt="Avatar" class="avatar-image"/>
        </div>
        
        <!-- Floating tech icons -->
        <div class="tech-icon tech-icon-react">
          <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <circle cx="12" cy="12" r="2" fill="#61DAFB"/>
            <ellipse cx="12" cy="12" rx="11" ry="4.2" stroke="#61DAFB" stroke-width="1" fill="none" opacity="0.8"/>
            <ellipse cx="12" cy="12" rx="11" ry="4.2" stroke="#61DAFB" stroke-width="1" fill="none" opacity="0.8" transform="rotate(60 12 12)"/>
            <ellipse cx="12" cy="12" rx="11" ry="4.2" stroke="#61DAFB" stroke-width="1" fill="none" opacity="0.8" transform="rotate(-60 12 12)"/>
          </svg>
        </div>
        
        <div class="tech-icon tech-icon-nextjs">
          <svg viewBox="0 0 394 80" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M261.919 0.0330723H330.547V12.7H303.323V79.339H289.71V12.7H261.919V0.0330723Z" fill="white"/>
            <path d="M149.052 0.0330723V12.7H94.0421V33.0772H138.281V45.7441H94.0421V66.6721H149.052V79.339H80.43V0.0330723H149.052Z" fill="white"/>
            <path d="M183.32 0.0661486H165.506L229.312 79.3721H247.178L215.271 39.7464L247.127 0.126654L229.312 0.154184L206.352 28.6697L183.32 0.0661486Z" fill="white"/>
            <path d="M201.6 56.7148L192.679 45.6229L165.455 79.4172H183.32L201.6 56.7148Z" fill="white"/>
            <path d="M80.907 79.4172L17.1 0.111206H0.439988L66.5232 79.4172H80.907Z" fill="white"/>
            <path d="M0.439988 0.111206L62.7501 66.9049L52.6162 79.4172H0.439988V0.111206Z" fill="white"/>
            <path d="M262.633 70.9335L340.216 0.0330723H363.546L304.563 70.9335H262.633Z" fill="white"/>
            <path d="M321.74 56.5743L309.378 45.6229L363.546 0.0330723H393.219L321.74 56.5743Z" fill="white"/>
          </svg>
        </div>
        
        <div class="tech-icon tech-icon-vue">
          <svg viewBox="0 0 128 128" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M78.8 10L64 41.9 49.2 10H0L64 128 128 10H78.8Z" fill="#41B883"/>
            <path d="M78.8 10L64 41.9 49.2 10H25.6L64 90.5 102.4 10H78.8Z" fill="#35495E"/>
          </svg>
        </div>
        
        <div class="tech-icon tech-icon-typescript">
          <div class="ts-icon">TS</div>
        </div>
        
        <div class="tech-icon tech-icon-database">
          <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <ellipse cx="12" cy="5" rx="9" ry="3" fill="#10B981" opacity="0.8"/>
            <path d="M3 5v14c0 1.66 4.03 3 9 3s9-1.34 9-3V5" fill="none" stroke="#10B981" stroke-width="2" stroke-linecap="round"/>
            <path d="M3 12c0 1.66 4.03 3 9 3s9-1.34 9-3" fill="none" stroke="#10B981" stroke-width="2" stroke-linecap="round"/>
          </svg>
        </div>
        
        <div class="tech-icon tech-icon-nodejs">
          <img src="/node.png"/>
        </div>
        
        <div class="tech-icon tech-icon-nestjs">
          <img src="/nestjs.png"/>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  texts: {
    type: Array,
    default: () => [
      'Welcome to my portfolio',
      'I create beautiful web experiences',
      'Let\'s build something amazing together'
    ]
  },
  typingSpeed: {
    type: Number,
    default: 100
  },
  deletingSpeed: {
    type: Number,
    default: 50
  },
  pauseTime: {
    type: Number,
    default: 2000
  }
})

const displayedText = ref('')
const showCursor = ref(true)
const currentTextIndex = ref(0)
const isDeleting = ref(false)
let typingInterval = null

const typeText = () => {
  const currentText = props.texts[currentTextIndex.value]
  
  if (!isDeleting.value) {
    // Typing forward
    if (displayedText.value.length < currentText.length) {
      displayedText.value = currentText.substring(0, displayedText.value.length + 1)
    } else {
      // Finished typing, wait then start deleting
      clearInterval(typingInterval)
      setTimeout(() => {
        isDeleting.value = true
        startTyping()
      }, props.pauseTime)
      return
    }
  } else {
    // Deleting backward
    if (displayedText.value.length > 0) {
      displayedText.value = currentText.substring(0, displayedText.value.length - 1)
    } else {
      // Finished deleting, move to next text
      isDeleting.value = false
      currentTextIndex.value = (currentTextIndex.value + 1) % props.texts.length
      clearInterval(typingInterval)
      setTimeout(() => {
        startTyping()
      }, 300)
      return
    }
  }
}

const startTyping = () => {
  const speed = isDeleting.value ? props.deletingSpeed : props.typingSpeed
  typingInterval = setInterval(typeText, speed)
}

onMounted(() => {
  startTyping()
  
  // Cursor blinking animation
  setInterval(() => {
    showCursor.value = !showCursor.value
  }, 530)
})

onUnmounted(() => {
  if (typingInterval) {
    clearInterval(typingInterval)
  }
})
</script>

<style scoped>
.banner {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 120px 2rem 4rem;
  background: #000000;
  position: relative;
  overflow: hidden;
}

.banner::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image: 
    radial-gradient(2px 2px at 15% 25%, rgba(255, 255, 255, 0.5), transparent),
    radial-gradient(1.5px 1.5px at 45% 65%, rgba(255, 255, 255, 0.4), transparent),
    radial-gradient(1px 1px at 35% 45%, rgba(255, 255, 255, 0.3), transparent),
    radial-gradient(2px 2px at 75% 15%, rgba(255, 255, 255, 0.5), transparent),
    radial-gradient(1px 1px at 85% 35%, rgba(255, 255, 255, 0.4), transparent),
    radial-gradient(1.5px 1.5px at 55% 85%, rgba(255, 255, 255, 0.3), transparent),
    radial-gradient(1px 1px at 25% 55%, rgba(255, 255, 255, 0.4), transparent),
    radial-gradient(2px 2px at 65% 75%, rgba(255, 255, 255, 0.5), transparent);
  background-repeat: repeat;
  background-size: 150% 150%;
  opacity: 0.4;
  animation: twinkle 25s linear infinite;
  pointer-events: none;
}

@keyframes twinkle {
  0% {
    background-position: 0% 0%;
    opacity: 0.4;
  }
  50% {
    opacity: 0.6;
  }
  100% {
    background-position: 100% 100%;
    opacity: 0.4;
  }
}

.banner-container {
  max-width: 1400px;
  margin: 0 auto;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.banner-content {
  text-align: left;
  color: white;
  z-index: 1;
  position: relative;
  
}

.banner-greeting {
  font-size: 1.5rem;
  font-weight: 400;
  margin-bottom: 1rem;
  opacity: 0.9;
  animation: fadeInUp 0.6s ease;
}

.banner-name {
  font-size: 4rem;
  font-weight: 700;
  margin-bottom: 0.5rem;
  line-height: 1.2;
  background: linear-gradient(135deg, #ffffff 0%, #e0e0e0 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: fadeInUp 0.8s ease 0.2s both;
}

.banner-profession {
  font-size: 1.25rem;
  margin-bottom: 2rem;
  opacity: 0.8;
  font-weight: 300;
  animation: fadeInUp 1s ease 0.4s both;
}

.banner-description {
  font-size: 1.5rem;
  min-height: 2rem;
  display: flex;
  align-items: center;
  gap: 0.25rem;
  animation: fadeInUp 1.2s ease 0.6s both;
}

.banner-avatar {
  position: relative;
  z-index: 9999999;
  flex-shrink: 0;
  width: 500px;
  height: 500px;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Purple glowing halo */
.avatar-halo {
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
}

@keyframes pulse {
  0%, 100% {
    transform: translate(-50%, -50%) scale(1);
    opacity: 0.8;
  }
  50% {
    transform: translate(-50%, -50%) scale(1.1);
    opacity: 1;
  }
}

/* Avatar image wrapper */
.avatar-image-wrapper {
  position: relative;
  z-index: 1;
  width: 400px;
  height: 400px;
  border-radius: 50%;
  overflow: hidden;
  background: transparent;
}

.avatar-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  filter: drop-shadow(0 10px 30px rgba(139, 92, 246, 0.3));
}

/* Tech icons floating around avatar */
.tech-icon {
  position: absolute;
  z-index: 3;
  width: 60px;
  height: 60px;
  background: rgba(139, 92, 246, 0.15);
  backdrop-filter: blur(10px);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(139, 92, 246, 0.3);
  animation: float 6s ease-in-out infinite;
}

.tech-icon svg {
  width: 40px;
  height: 40px;
}

.tech-icon-nextjs svg {
  width: 45px;
  height: 35px;
}

.tech-icon-react {
  top: 5%;
  left: 10%;
  animation-delay: 0s;
}

.tech-icon-nextjs {
  bottom: 5%;
  left: 10%;
  animation-delay: 2s;
}

.tech-icon-vue {
  top: 5%;
  right: 10%;
  animation-delay: 1s;
}

.tech-icon-typescript {
  bottom: 5%;
  right: 10%;
  animation-delay: 3s;
}

.tech-icon-database {
  left: -5%;
  top: 50%;
  transform: translateY(-50%);
  animation-delay: 1.5s;
}

.tech-icon-nodejs {
  top: 25%;
  right: -5%;
  animation-delay: 0.5s;
}

.tech-icon-nestjs {
  bottom: 25%;
  right: -5%;
  animation-delay: 2.5s;
}

.ts-icon {
  font-size: 24px;
  font-weight: 700;
  color: #3178C6;
  letter-spacing: -1px;
}

@keyframes float {
  0%, 100% {
    transform: translateY(0) rotate(0deg);
  }
  33% {
    transform: translateY(-10px) rotate(5deg);
  }
  66% {
    transform: translateY(10px) rotate(-5deg);
  }
}

.typing-text {
  color: rgba(255, 255, 255, 0.9);
  font-weight: 400;
}

.cursor {
  color: #ffffff;
  font-weight: 300;
  animation: blink 1s infinite;
}

.cursor.blinking {
  opacity: 1;
}

@keyframes blink {
  0%, 50% {
    opacity: 1;
  }
  51%, 100% {
    opacity: 0;
  }
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
@media (max-width: 1024px) {
  .banner-container {
    flex-direction: column;
    gap: 3rem;
    text-align: center;
  }
  
  .banner-content {
    text-align: center;
  }
  
  .banner-avatar {
    width: 420px;
    height: 420px;
  }
  
  .avatar-halo {
    width: 420px;
    height: 420px;
  }
  
  .avatar-image-wrapper {
    width: 360px;
    height: 360px;
  }
  
  .tech-icon {
    width: 56px;
    height: 56px;
  }
  
  .tech-icon svg {
    width: 38px;
    height: 38px;
  }
  
  .ts-icon {
    font-size: 22px;
  }
}

@media (max-width: 768px) {
  .banner {
    padding: 100px 1.5rem 3rem;
  }

  .banner-name {
    font-size: 2.5rem;
  }

  .banner-greeting {
    font-size: 1.25rem;
  }

  .banner-profession {
    font-size: 1rem;
  }

  .banner-description {
    font-size: 1.25rem;
  }
  
  .banner-avatar {
    width: 320px;
    height: 320px;
  }
  
  .avatar-halo {
    width: 320px;
    height: 320px;
  }
  
  .avatar-image-wrapper {
    width: 270px;
    height: 270px;
  }
  
  .tech-icon {
    width: 50px;
    height: 50px;
  }
  
  .tech-icon-react {
    top: 4%;
    left: 6%;
  }
  
  .tech-icon-nextjs {
    bottom: 4%;
    left: 6%;
  }
  
  .tech-icon-vue {
    top: 4%;
    right: 6%;
  }
  
  .tech-icon-typescript {
    bottom: 4%;
    right: 6%;
  }
  
  .tech-icon-database {
    left: -3%;
    top: 50%;
    transform: translateY(-50%);
  }
  
  .tech-icon-nodejs {
    top: 20%;
    right: -3%;
  }
  
  .tech-icon-nestjs {
    bottom: 20%;
    right: -3%;
  }
}

@media (max-width: 480px) {
  .banner-name {
    font-size: 2rem;
  }

  .banner-description {
    font-size: 1rem;
  }
  
  .banner-avatar {
    width: 270px;
    height: 270px;
  }
  
  .avatar-halo {
    width: 270px;
    height: 270px;
  }
  
  .avatar-image-wrapper {
    width: 230px;
    height: 230px;
  }
  
  .tech-icon {
    width: 44px;
    height: 44px;
  }
  
  .tech-icon svg {
    width: 30px;
    height: 30px;
  }
  
  .ts-icon {
    font-size: 19px;
  }
}
</style>
