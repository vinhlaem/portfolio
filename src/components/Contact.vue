<script setup>
import { reactive, ref, computed } from "vue";
import emailjs from "@emailjs/browser";

const SERVICE_ID = import.meta.env.VITE_EMAILJS_SERVICE_ID || "";
const TEMPLATE_ID = import.meta.env.VITE_EMAILJS_TEMPLATE_ID || "";
const PUBLIC_KEY = import.meta.env.VITE_EMAILJS_PUBLIC_KEY || "";

const form = reactive({
  subject: "Email from Portfolio",
  name: "",
  email: "",
  number: "",
  message: "",
});

const isValidEmail = (value) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value.trim());
const isValidPhone = (value) => {
  const digits = value.replace(/[^0-9]/g, "");
  return digits.length >= 8 && digits.length <= 15;
};

const sending = ref(false);
const status = ref({ type: "", message: "" });

const canSubmit = computed(
  () =>
    form.subject.trim() &&
    form.name.trim() &&
    form.email.trim() &&
    form.message.trim() &&
    form.number.trim() &&
    isValidEmail(form.email) &&
    isValidPhone(form.number) &&
    !sending.value
);

const sendEmail = async () => {
  status.value = { type: "", message: "" };

  if (!SERVICE_ID || !TEMPLATE_ID || !PUBLIC_KEY) {
    status.value = {
      type: "error",
      message: "Missing EmailJS keys. Add VITE_EMAILJS_* env vars.",
    };
    return;
  }

  if (!isValidEmail(form.email)) {
    status.value = { type: "error", message: "Email is not valid." };
    return;
  }

  if (!isValidPhone(form.number)) {
    status.value = {
      type: "error",
      message: "Phone number is not valid (8-15 digits).",
    };
    return;
  }

  sending.value = true;
  try {
    await emailjs.send(
      SERVICE_ID,
      TEMPLATE_ID,
      {
        subject: form.subject,
        name: form.name,
        email: form.email,
        number: form.number,
        message: form.message,
      },
      { publicKey: PUBLIC_KEY }
    );
    status.value = {
      type: "success",
      message: "Sent successfully. I will reply soon!",
    };
    form.name = "";
    form.email = "";
    form.number = "";
    form.message = "";
  } catch (err) {
    console.error(err);
    status.value = { type: "error", message: "Send failed. Please try again." };
  } finally {
    sending.value = false;
  }
};
</script>

<template>
  <section id="contact" class="contact-section">
    <div class="section-header">
      <div>
        <p class="eyebrow">Contact</p>
        <h2>Let’s build something great</h2>
        <p class="subtitle">
          Tell me about your project or just say hello. I usually respond within
          24 hours.
        </p>
      </div>
    </div>

    <div class="card">
      <form class="form" @submit.prevent="sendEmail">
        <div class="field">
          <label for="name">Name</label>
          <input
            id="name"
            v-model="form.name"
            type="text"
            name="name"
            placeholder="Your name"
            required
          />
        </div>
        <div class="field">
          <label for="email">Email</label>
          <input
            id="email"
            v-model="form.email"
            type="email"
            name="email"
            placeholder="you@example.com"
            required
            :class="{ invalid: form.email && !isValidEmail(form.email) }"
          />
          <small
            v-if="form.email && !isValidEmail(form.email)"
            class="helper error"
          >
            Please enter a valid email.
          </small>
        </div>
        <div class="field">
          <label for="number">Phone Number</label>
          <input
            id="number"
            v-model="form.number"
            type="tel"
            inputmode="tel"
            name="number"
            placeholder="Your phone number"
            required
            :class="{ invalid: form.number && !isValidPhone(form.number) }"
          />
          <small
            v-if="form.number && !isValidPhone(form.number)"
            class="helper error"
          >
            8-15 digits, numbers only.
          </small>
        </div>
        <div class="field">
          <label for="message">Message</label>
          <textarea
            id="message"
            v-model="form.message"
            name="message"
            rows="4"
            placeholder="Tell me about your idea..."
            required
          />
        </div>

        <button class="submit" type="submit" :disabled="!canSubmit">
          <span v-if="sending">Sending...</span>
          <span v-else>Send message</span>
        </button>

        <p v-if="status.message" :class="['status', status.type]">
          {{ status.message }}
        </p>
      </form>
      <div class="aside">
        <a href="https://github.com/vinhlaem" target="_blank">
          <svg
            width="50px"
            height="50px"
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
            width="50px"
            height="50px"
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
            width="50px"
            height="50px"
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
            width="50px"
            height="50px"
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
  </section>
</template>

<style scoped>
.contact-section {
  padding: 80px 2rem;
}

.section-header {
  max-width: 1400px;
  margin: 0 auto 28px;
  position: relative;
  z-index: 9999999;
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
  color: #c6c9d4;
  margin: 0;
  line-height: 1.5;
  max-width: 620px;
}

.card {
  max-width: 1400px;
  margin: 0 auto;

  gap: 16px;
  display: flex;
  justify-content: space-between;
  align-items: start;
  backdrop-filter: blur(10px);
  position: relative;
  z-index: 9999999;
}

.form {
  display: grid;
  gap: 14px;
  position: relative;
  z-index: 9999999;
  width: 50%;
  background: rgba(0, 0, 0, 0.5);
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 18px;
  padding: 20px;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.45);
}

.field {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

label {
  font-weight: 600;
  color: #e7e9f3;
}

input,
textarea {
  width: 100%;
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 12px;
  padding: 12px 14px;
  color: #fff;
  font-size: 1rem;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: rgba(109, 75, 255, 0.8);
  box-shadow: 0 0 0 3px rgba(109, 75, 255, 0.15);
}

.invalid {
  border-color: rgba(248, 113, 113, 0.7);
  box-shadow: 0 0 0 3px rgba(248, 113, 113, 0.18);
}

.helper {
  color: #c6c9d4;
  font-size: 0.9rem;
  margin: 0;
}

.helper.error {
  color: #ffb6b6;
}

textarea {
  resize: vertical;
  min-height: 140px;
}

.submit {
  border: none;
  background: linear-gradient(135deg, #6d4bff, #8a7bff);
  color: #fff;
  padding: 12px 16px;
  border-radius: 12px;
  font-weight: 700;
  cursor: pointer;
  transition: transform 0.15s ease, box-shadow 0.15s ease;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-height: 48px;
}

.submit:hover {
  transform: translateY(-1px);
  box-shadow: 0 12px 28px rgba(109, 75, 255, 0.35);
}

.submit:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

.status {
  margin: 0;
  font-weight: 600;
  padding: 10px 12px;
  border-radius: 10px;
}

.status.success {
  background: rgba(34, 197, 94, 0.15);
  color: #c4f3d6;
  border: 1px solid rgba(34, 197, 94, 0.25);
}

.status.error {
  background: rgba(248, 113, 113, 0.18);
  color: #ffd5d5;
  border: 1px solid rgba(248, 113, 113, 0.25);
}

.aside {
  border-radius: 14px;
  padding: 16px;
  display: grid;
  gap: 10px;
  width: 50%;
  display: grid;
  gap: 10px;
  grid-template-columns: 2fr 2fr;
}
.aside a {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100px;
  padding: 40px 0;
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.06);
}

.aside a:hover {
  border: 1px solid rgba(109, 75, 255, 0.5);
  transform: translateY(-3px);
  box-shadow: 0 12px 28px rgba(109, 75, 255, 0.35);
  transition: transform 0.2s ease, border-color 0.2s ease, box-shadow 0.2s ease;
}

.chip {
  display: inline-flex;
  padding: 8px 12px;
  border-radius: 999px;
  background: rgba(109, 75, 255, 0.16);
  border: 1px solid rgba(109, 75, 255, 0.25);
  color: #e2ddff;
  font-weight: 700;
  width: fit-content;
}

.meta {
  color: #c6c9d4;
  font-size: 0.95rem;
}

pre {
  background: rgba(255, 255, 255, 0.04);
  padding: 10px;
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.06);
  color: #dbe0ff;
  font-family: "SFMono-Regular", Consolas, "Liberation Mono", Menlo, monospace;
  font-size: 0.9rem;
  margin: 8px 0 0;
}

@media (max-width: 960px) {
  h2 {
    font-size: 2.2rem;
  }

  .card {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 768px) {
  .contact-section {
    padding: 60px 1.2rem;
  }

  .card {
    flex-direction: column;
  }

  .form {
    width: 100%;
  }

  .aside {
    width: 100%;
  }

  .card {
    padding: 16px;
  }
}

@media (max-width: 375px) {
  .aside svg {
    width: 40px;
    height: 40px;
  }
}
</style>
