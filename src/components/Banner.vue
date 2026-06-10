<template>
  <section id="home" class="banner">
    <div class="banner-container">
      <div class="banner-content">
        <h2 class="banner-greeting">👋 Greetings!</h2>
        <h1 class="banner-name">Vinh Truong</h1>
        <p class="banner-profession">FullStack developer</p>
        <div class="banner-description">
          <span class="typing-text">{{ displayedText }}</span>
          <span class="cursor">|</span>
        </div>
        <div class="banner-social-links">
          <a href="https://github.com/vinhlaem" target="_blank">
            <svg
              width="30px"
              height="30px"
              viewBox="0 0 24 24"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <g stroke-width="0"></g>
              <g
                id="SVGRepo_tracerCarrier"
                stroke-linecap="round"
                stroke-linejoin="round"
              ></g>
              <g id="SVGRepo_iconCarrier">
                <path
                  d="M18.6713 2.62664C18.5628 2.36483 18.3534 2.16452 18.0959 2.07627L18.094 2.07564L18.0922 2.07501L18.0884 2.07374L18.0805 2.07114L18.0636 2.06583C18.0518 2.06223 18.039 2.05856 18.0252 2.05487C17.9976 2.04749 17.966 2.04007 17.9305 2.03319C17.8593 2.01941 17.7728 2.00787 17.6708 2.00279C17.466 1.99259 17.2037 2.00858 16.8817 2.08054C16.3447 2.20053 15.6476 2.47464 14.7724 3.03631C14.7152 3.07302 14.6572 3.11096 14.5985 3.15016C14.5397 3.13561 14.4809 3.12155 14.422 3.108C12.8261 2.74083 11.1742 2.74083 9.57825 3.108C9.51933 3.12156 9.46049 3.13561 9.40173 3.15017C9.34298 3.11096 9.28499 3.07302 9.22775 3.03631C8.35163 2.47435 7.65291 2.20029 7.11455 2.08039C6.79179 2.00852 6.52891 1.99262 6.324 2.00278C6.22186 2.00784 6.13536 2.01931 6.06428 2.03299C6.0288 2.03982 5.99732 2.04717 5.96983 2.05447C5.95609 2.05812 5.94336 2.06176 5.93163 2.06531L5.91481 2.07056L5.90698 2.07311L5.9032 2.07437L5.90135 2.07499L5.89952 2.07561C5.63979 2.16397 5.42877 2.36623 5.32049 2.63061C4.91716 3.6154 4.8101 4.70134 5.00435 5.74306C5.01379 5.79367 5.02394 5.84418 5.0348 5.89458C4.99316 5.95373 4.9527 6.01368 4.91343 6.07439C4.30771 7.01089 3.98553 8.12791 4.00063 9.27493C4.00208 11.7315 4.71965 13.4139 5.9332 14.4965C6.62014 15.1093 7.41743 15.4844 8.21873 15.7208C8.31042 15.7479 8.40217 15.7731 8.49381 15.7967C8.48043 15.8432 8.46796 15.8901 8.45641 15.9373C8.40789 16.1357 8.37572 16.3394 8.36083 16.5461C8.35948 16.5648 8.35863 16.5835 8.35829 16.6022L8.32436 18.421L8.32417 18.4407C8.32417 18.4464 8.32417 18.4521 8.32417 18.4577C8.26262 18.473 8.20005 18.4843 8.13682 18.4916C7.942 18.5141 7.74467 18.4977 7.5561 18.4434C7.36752 18.3891 7.19127 18.2979 7.03752 18.1749C6.88377 18.0519 6.75553 17.8994 6.66031 17.7261L6.6505 17.7087C6.38836 17.2535 6.02627 16.8639 5.59142 16.5695C5.15656 16.275 4.6604 16.0836 4.14047 16.0099C3.59365 15.9324 3.08753 16.3128 3.01002 16.8597C2.93251 17.4065 3.31296 17.9126 3.85978 17.9901C4.07816 18.0211 4.28688 18.1015 4.47012 18.2256C4.65121 18.3482 4.80277 18.5103 4.9134 18.7C5.1346 19.0992 5.43165 19.4514 5.78801 19.7365C6.14753 20.0242 6.56032 20.2379 7.00272 20.3653C7.43348 20.4893 7.88392 20.5291 8.32949 20.4825C8.33039 20.7224 8.33103 20.9065 8.33103 21C8.33103 21.5523 8.75521 22 9.27847 22H14.7558C15.279 22 15.7032 21.5523 15.7032 21V17.2095C15.729 16.7802 15.685 16.3499 15.5738 15.9373C15.5585 15.8805 15.5419 15.824 15.5241 15.7679C15.5838 15.753 15.6435 15.7373 15.7032 15.7208C16.5277 15.4937 17.3513 15.1224 18.0588 14.4983C19.2791 13.4217 19.9982 11.7379 19.9996 9.27493C20.0147 8.12791 19.6925 7.01089 19.0868 6.07439C19.0475 6.01358 19.007 5.95354 18.9652 5.89429C18.976 5.84399 18.9861 5.79358 18.9955 5.74306C19.1893 4.69934 19.0795 3.61142 18.6713 2.62664Z"
                  fill="#ffffff"
                ></path>
              </g>
            </svg>
          </a>
          <a
            href="mailto:vinhlaem20000@gmail.com"
            title="vinhlaem20000@gmail.com"
            target="_blank"
          >
            <svg
              fill="#ffffff"
              width="30px"
              height="30px"
              viewBox="0 0 32 32"
              version="1.1"
              xmlns="http://www.w3.org/2000/svg"
              stroke="#ffffff"
            >
              <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
              <g
                id="SVGRepo_tracerCarrier"
                stroke-linecap="round"
                stroke-linejoin="round"
              ></g>
              <g id="SVGRepo_iconCarrier">
                <title>gmail</title>
                <path
                  d="M30.996 7.824v17.381c0 0 0 0 0 0.001 0 1.129-0.915 2.044-2.044 2.044-0 0-0 0-0.001 0h-4.772v-11.587l-8.179 6.136-8.179-6.136v11.588h-4.772c0 0 0 0-0 0-1.129 0-2.044-0.915-2.044-2.044 0-0 0-0.001 0-0.001v0-17.381c0-0 0-0.001 0-0.001 0-1.694 1.373-3.067 3.067-3.067 0.694 0 1.334 0.231 1.848 0.619l-0.008-0.006 10.088 7.567 10.088-7.567c0.506-0.383 1.146-0.613 1.84-0.613 1.694 0 3.067 1.373 3.067 3.067v0z"
                ></path>
              </g>
            </svg>
          </a>
          <a href="tel:+84769980620" title="0769980620" target="_blank">
            <svg
              width="30px"
              height="30px"
              viewBox="0 0 16 16"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <g stroke-width="0"></g>
              <g
                id="SVGRepo_tracerCarrier"
                stroke-linecap="round"
                stroke-linejoin="round"
              ></g>
              <g id="SVGRepo_iconCarrier">
                <path
                  d="M1 5V1H7V5L4.5 7.5L8.5 11.5L11 9H15V15H11C5.47715 15 1 10.5228 1 5Z"
                  fill="#ffffff"
                ></path>
              </g>
            </svg>
          </a>
          <a
            href="/TruongDinhVinh.pdf"
            download="TruongDinhVinh.pdf"
            target="_blank"
          >
            <svg
              fill="#ffffff"
              version="1.1"
              id="Capa_1"
              xmlns="http://www.w3.org/2000/svg"
              xmlns:xlink="http://www.w3.org/1999/xlink"
              width="30px"
              height="30px"
              viewBox="0 0 45.057 45.057"
              xml:space="preserve"
              stroke="#ffffff"
            >
              <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
              <g
                id="SVGRepo_tracerCarrier"
                stroke-linecap="round"
                stroke-linejoin="round"
              ></g>
              <g id="SVGRepo_iconCarrier">
                <g>
                  <g id="_x35_8_24_">
                    <g>
                      <path
                        d="M19.558,25.389c-0.067,0.176-0.155,0.328-0.264,0.455c-0.108,0.129-0.24,0.229-0.396,0.301 c-0.156,0.072-0.347,0.107-0.57,0.107c-0.313,0-0.572-0.068-0.78-0.203c-0.208-0.137-0.374-0.316-0.498-0.541 c-0.124-0.223-0.214-0.477-0.27-0.756c-0.057-0.279-0.084-0.564-0.084-0.852c0-0.289,0.027-0.572,0.084-0.853 c0.056-0.281,0.146-0.533,0.27-0.756c0.124-0.225,0.29-0.404,0.498-0.541c0.208-0.137,0.468-0.203,0.78-0.203 c0.271,0,0.494,0.051,0.666,0.154c0.172,0.105,0.31,0.225,0.414,0.361c0.104,0.137,0.176,0.273,0.216,0.414 c0.04,0.139,0.068,0.25,0.084,0.33h2.568c-0.112-1.08-0.49-1.914-1.135-2.502c-0.644-0.588-1.558-0.887-2.741-0.895 c-0.664,0-1.263,0.107-1.794,0.324c-0.532,0.215-0.988,0.52-1.368,0.912c-0.38,0.392-0.672,0.863-0.876,1.416 c-0.204,0.551-0.307,1.165-0.307,1.836c0,0.631,0.097,1.223,0.288,1.77c0.192,0.549,0.475,1.021,0.847,1.422 s0.825,0.717,1.361,0.949c0.536,0.23,1.152,0.348,1.849,0.348c0.624,0,1.18-0.105,1.668-0.312 c0.487-0.209,0.897-0.482,1.229-0.822s0.584-0.723,0.756-1.146c0.172-0.422,0.259-0.852,0.259-1.283h-2.593 C19.68,25.023,19.627,25.214,19.558,25.389z"
                      ></path>
                      <polygon
                        points="26.62,24.812 26.596,24.812 25.192,19.616 22.528,19.616 25.084,28.184 28.036,28.184 30.713,19.616 28,19.616 "
                      ></polygon>
                      <path
                        d="M33.431,0H5.179v45.057h34.699V6.251L33.431,0z M36.878,42.056H8.179V3h23.706v4.76h4.992L36.878,42.056L36.878,42.056z"
                      ></path>
                    </g>
                  </g>
                </g>
              </g>
            </svg>
          </a>
        </div>
      </div>

      <div class="banner-avatar">
        <!-- Purple glowing halo behind avatar -->
        <div class="avatar-halo"></div>

        <!-- Avatar image -->
        <div class="avatar-image-wrapper">
          <img
            src="/optimized/avatar-480.jpg"
            srcset="/optimized/avatar-320.jpg 320w, /optimized/avatar-480.jpg 480w, /optimized/avatar-754.jpg 754w"
            sizes="(max-width: 480px) 230px, (max-width: 768px) 270px, (max-width: 1024px) 360px, 400px"
            alt="Avatar"
            class="avatar-image"
            width="754"
            height="552"
            decoding="async"
            fetchpriority="high"
          />
        </div>

        <!-- Floating tech icons -->
        <div class="tech-icon tech-icon-react">
          <svg
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <circle cx="12" cy="12" r="2" fill="#61DAFB" />
            <ellipse
              cx="12"
              cy="12"
              rx="11"
              ry="4.2"
              stroke="#61DAFB"
              stroke-width="1"
              fill="none"
              opacity="0.8"
            />
            <ellipse
              cx="12"
              cy="12"
              rx="11"
              ry="4.2"
              stroke="#61DAFB"
              stroke-width="1"
              fill="none"
              opacity="0.8"
              transform="rotate(60 12 12)"
            />
            <ellipse
              cx="12"
              cy="12"
              rx="11"
              ry="4.2"
              stroke="#61DAFB"
              stroke-width="1"
              fill="none"
              opacity="0.8"
              transform="rotate(-60 12 12)"
            />
          </svg>
        </div>

        <div class="tech-icon tech-icon-nextjs">
          <svg
            viewBox="0 0 394 80"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M261.919 0.0330723H330.547V12.7H303.323V79.339H289.71V12.7H261.919V0.0330723Z"
              fill="white"
            />
            <path
              d="M149.052 0.0330723V12.7H94.0421V33.0772H138.281V45.7441H94.0421V66.6721H149.052V79.339H80.43V0.0330723H149.052Z"
              fill="white"
            />
            <path
              d="M183.32 0.0661486H165.506L229.312 79.3721H247.178L215.271 39.7464L247.127 0.126654L229.312 0.154184L206.352 28.6697L183.32 0.0661486Z"
              fill="white"
            />
            <path
              d="M201.6 56.7148L192.679 45.6229L165.455 79.4172H183.32L201.6 56.7148Z"
              fill="white"
            />
            <path
              d="M80.907 79.4172L17.1 0.111206H0.439988L66.5232 79.4172H80.907Z"
              fill="white"
            />
            <path
              d="M0.439988 0.111206L62.7501 66.9049L52.6162 79.4172H0.439988V0.111206Z"
              fill="white"
            />
            <path
              d="M262.633 70.9335L340.216 0.0330723H363.546L304.563 70.9335H262.633Z"
              fill="white"
            />
            <path
              d="M321.74 56.5743L309.378 45.6229L363.546 0.0330723H393.219L321.74 56.5743Z"
              fill="white"
            />
          </svg>
        </div>

        <div class="tech-icon tech-icon-vue">
          <svg
            viewBox="0 0 128 128"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M78.8 10L64 41.9 49.2 10H0L64 128 128 10H78.8Z"
              fill="#41B883"
            />
            <path
              d="M78.8 10L64 41.9 49.2 10H25.6L64 90.5 102.4 10H78.8Z"
              fill="#35495E"
            />
          </svg>
        </div>

        <div class="tech-icon tech-icon-typescript">
          <div class="ts-icon">TS</div>
        </div>

        <div class="tech-icon tech-icon-database">
          <svg
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <ellipse
              cx="12"
              cy="5"
              rx="9"
              ry="3"
              fill="#10B981"
              opacity="0.8"
            />
            <path
              d="M3 5v14c0 1.66 4.03 3 9 3s9-1.34 9-3V5"
              fill="none"
              stroke="#10B981"
              stroke-width="2"
              stroke-linecap="round"
            />
            <path
              d="M3 12c0 1.66 4.03 3 9 3s9-1.34 9-3"
              fill="none"
              stroke="#10B981"
              stroke-width="2"
              stroke-linecap="round"
            />
          </svg>
        </div>

        <div class="tech-icon tech-icon-nodejs">
          <img src="/node.png" alt="Node.js" loading="lazy" decoding="async" />
        </div>

        <div class="tech-icon tech-icon-nestjs">
          <img src="/nestjs.png" alt="NestJS" loading="lazy" decoding="async" />
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const props = defineProps({
  texts: {
    type: Array,
    default: () => [
      "Welcome to my portfolio",
      "I create beautiful web experiences",
      "Let's build something amazing together",
    ],
  },
  typingSpeed: {
    type: Number,
    default: 100,
  },
  deletingSpeed: {
    type: Number,
    default: 50,
  },
  pauseTime: {
    type: Number,
    default: 2000,
  },
});

const displayedText = ref(props.texts[0] || "");
const currentTextIndex = ref(0);
const isDeleting = ref(false);
let typingInterval = null;
let typingTimeout = null;
let typingStartTimeout = null;

const typeText = () => {
  const currentText = props.texts[currentTextIndex.value];

  if (!isDeleting.value) {
    // Typing forward
    if (displayedText.value.length < currentText.length) {
      displayedText.value = currentText.substring(
        0,
        displayedText.value.length + 1
      );
    } else {
      // Finished typing, wait then start deleting
      clearInterval(typingInterval);
      typingTimeout = setTimeout(() => {
        isDeleting.value = true;
        startTyping();
      }, props.pauseTime);
      return;
    }
  } else {
    // Deleting backward
    if (displayedText.value.length > 0) {
      displayedText.value = currentText.substring(
        0,
        displayedText.value.length - 1
      );
    } else {
      // Finished deleting, move to next text
      isDeleting.value = false;
      currentTextIndex.value =
        (currentTextIndex.value + 1) % props.texts.length;
      clearInterval(typingInterval);
      typingTimeout = setTimeout(() => {
        startTyping();
      }, 300);
      return;
    }
  }
};

const startTyping = () => {
  clearInterval(typingInterval);
  const speed = isDeleting.value ? props.deletingSpeed : props.typingSpeed;
  typingInterval = setInterval(typeText, speed);
};

onMounted(() => {
  typingStartTimeout = window.setTimeout(() => {
    startTyping();
  }, 2500);
});

onUnmounted(() => {
  if (typingInterval) {
    clearInterval(typingInterval);
  }
  if (typingTimeout) {
    clearTimeout(typingTimeout);
  }
  if (typingStartTimeout) {
    clearTimeout(typingStartTimeout);
  }
});
</script>

<style scoped>
.banner {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 120px 2rem 4rem;
  background: transparent;
  position: relative;
  overflow: hidden;
}

@media (prefers-reduced-motion: reduce) {
  .banner::before,
  .banner-greeting,
  .banner-name,
  .banner-profession,
  .banner-description,
  .avatar-halo,
  .tech-icon,
  .cursor {
    animation: none;
  }
}

.banner::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image: radial-gradient(
      2px 2px at 15% 25%,
      rgba(255, 255, 255, 0.5),
      transparent
    ),
    radial-gradient(
      1.5px 1.5px at 45% 65%,
      rgba(255, 255, 255, 0.4),
      transparent
    ),
    radial-gradient(1px 1px at 35% 45%, rgba(255, 255, 255, 0.3), transparent),
    radial-gradient(2px 2px at 75% 15%, rgba(255, 255, 255, 0.5), transparent),
    radial-gradient(1px 1px at 85% 35%, rgba(255, 255, 255, 0.4), transparent),
    radial-gradient(
      1.5px 1.5px at 55% 85%,
      rgba(255, 255, 255, 0.3),
      transparent
    ),
    radial-gradient(1px 1px at 25% 55%, rgba(255, 255, 255, 0.4), transparent),
    radial-gradient(2px 2px at 65% 75%, rgba(255, 255, 255, 0.5), transparent);
  background-repeat: repeat;
  background-size: 150% 150%;
  opacity: 0.4;
  animation: none;
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
  animation: none;
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
  animation: none;
}

.banner-profession {
  font-size: 1.25rem;
  margin-bottom: 2rem;
  opacity: 0.8;
  font-weight: 300;
  animation: none;
}

.banner-description {
  font-size: 1.5rem;
  min-height: 2rem;
  display: flex;
  align-items: center;
  gap: 0.25rem;
  animation: none;
}

.banner-social-links {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-top: 1rem;
}

.banner-social-links a {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 30px;
  height: 30px;
}

.banner-avatar {
  position: relative;
  z-index: 2;
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
  background: radial-gradient(
    circle,
    rgba(139, 92, 246, 0.6) 0%,
    rgba(139, 92, 246, 0.4) 40%,
    rgba(139, 92, 246, 0.1) 70%,
    transparent 100%
  );
  filter: blur(24px);
  animation: none;
  z-index: 0;
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
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(139, 92, 246, 0.3);
  animation: none;
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
  color: #3178c6;
  letter-spacing: -1px;
}

@keyframes float {
  0%,
  100% {
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
  animation: none;
}

.cursor.blinking {
  opacity: 1;
}

@keyframes blink {
  0%,
  50% {
    opacity: 1;
  }
  51%,
  100% {
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
  .banner-social-links {
    justify-content: center;
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
