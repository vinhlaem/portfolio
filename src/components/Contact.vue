<script setup>
import { reactive, ref, computed } from 'vue'
import emailjs from '@emailjs/browser'

const SERVICE_ID = import.meta.env.VITE_EMAILJS_SERVICE_ID || ''
const TEMPLATE_ID = import.meta.env.VITE_EMAILJS_TEMPLATE_ID || ''
const PUBLIC_KEY = import.meta.env.VITE_EMAILJS_PUBLIC_KEY || ''

const form = reactive({
  subject: 'Email from Portfolio',
  name: '',
  email: '',
  number: '',
  message: ''
})

const isValidEmail = (value) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value.trim())
const isValidPhone = (value) => {
  const digits = value.replace(/[^0-9]/g, '')
  return digits.length >= 8 && digits.length <= 15
}

const sending = ref(false)
const status = ref({ type: '', message: '' })

const canSubmit = computed(() =>
  form.subject.trim() &&
  form.name.trim() &&
  form.email.trim() &&
  form.message.trim() &&
  form.number.trim() &&
  isValidEmail(form.email) &&
  isValidPhone(form.number) &&
  !sending.value
)

const sendEmail = async () => {
  status.value = { type: '', message: '' }

  if (!SERVICE_ID || !TEMPLATE_ID || !PUBLIC_KEY) {
    status.value = { type: 'error', message: 'Missing EmailJS keys. Add VITE_EMAILJS_* env vars.' }
    return
  }

  if (!isValidEmail(form.email)) {
    status.value = { type: 'error', message: 'Email is not valid.' }
    return
  }

  if (!isValidPhone(form.number)) {
    status.value = { type: 'error', message: 'Phone number is not valid (8-15 digits).' }
    return
  }

  sending.value = true
  try {
    await emailjs.send(
      SERVICE_ID,
      TEMPLATE_ID,
      {
        subject: form.subject,
        name: form.name,
        email: form.email,
        number: form.number,
        message: form.message
      },
      { publicKey: PUBLIC_KEY }
    )
    status.value = { type: 'success', message: 'Sent successfully. I will reply soon!' }
    form.name = ''
    form.email = ''
    form.number = ''
    form.message = ''
  } catch (err) {
    console.error(err)
    status.value = { type: 'error', message: 'Send failed. Please try again.' }
  } finally {
    sending.value = false
  }
}
</script>

<template>
  <section id="contact" class="contact-section">
    <div class="section-header">
      <div>
        <p class="eyebrow">Contact</p>
        <h2>Let’s build something great</h2>
        <p class="subtitle">
          Tell me about your project or just say hello. I usually respond within 24 hours.
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
          <small v-if="form.email && !isValidEmail(form.email)" class="helper error">
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
          <small v-if="form.number && !isValidPhone(form.number)" class="helper error">
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
  background: rgba(0, 0, 0, 0.5);
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 18px;
  padding: 20px;
  gap: 16px;
  backdrop-filter: blur(10px);
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.45);
  position: relative;
  z-index: 9999999;
}

.form {
  display: grid;
  gap: 14px;
  position: relative;
  z-index: 9999999;
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
  background: rgba(0, 0, 0, 0.35);
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 14px;
  padding: 16px;
  display: grid;
  gap: 10px;
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
  font-family: 'SFMono-Regular', Consolas, 'Liberation Mono', Menlo, monospace;
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

@media (max-width: 600px) {
  .contact-section {
    padding: 60px 1.2rem;
  }

  .card {
    padding: 16px;
  }
}
</style>

