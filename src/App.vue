<template>
  <div class="app">
    <div class="form-container">
      <h1>💌 Contact Us</h1>
      <form @submit.prevent="submitForm">
        <label>Name:</label>
        <input type="text" v-model="form.name" placeholder="Your Name" />

        <label>Email:</label>
        <input type="email" v-model="form.email" placeholder="you@example.com" />

        <label>Message:</label>
        <textarea v-model="form.message" placeholder="Your message..."></textarea>

        <button type="submit" class="blinking">✨ Submit ✨</button>
      </form>

      <p v-if="submitted" class="thanks-msg">
        ✅ Thank you {{ lastSubmittedName }}! We received your message.
      </p>
    </div>

    <div v-if="storedMessages.length" class="messages">
      <h2>📬 Previous Messages</h2>
      <ul>
        <li v-for="(msg, index) in storedMessages" :key="index">
          <p><strong>{{ msg.name }}</strong> ({{ msg.email }})</p>
          <p>{{ msg.message }}</p>
          <small>🕒 {{ msg.submittedAt }}</small>
          <hr />
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const form = ref({ name: '', email: '', message: '' })
const submitted = ref(false)
const lastSubmittedName = ref('')
const storedMessages = ref([])

const submitForm = () => {
  if (!form.value.name || !form.value.email || !form.value.message) {
    alert('Please fill all fields!')
    return
  }

  const newMessage = {
    ...form.value,
    submittedAt: new Date().toLocaleString()
  }

  const messages = JSON.parse(localStorage.getItem('formMessages')) || []
  messages.push(newMessage)
  localStorage.setItem('formMessages', JSON.stringify(messages))

  lastSubmittedName.value = form.value.name
  form.value.name = ''
  form.value.email = ''
  form.value.message = ''
  submitted.value = true
  storedMessages.value = messages
}

onMounted(() => {
  const messages = localStorage.getItem('formMessages')
  if (messages) storedMessages.value = JSON.parse(messages)
})
</script>

<style scoped>
/* Background image and gradient */
.app {
  min-height: 100vh;
  background: linear-gradient(to right, #74ebd5, #acb6e5), url('https://images.unsplash.com/photo-1503264116251-35a269479413?auto=format&fit=crop&w=1500&q=80') no-repeat center center/cover;
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: 'Segoe UI', sans-serif;
  padding: 2rem;
  color: #333;
}

/* Form container */
.form-container {
  background-color: rgba(255, 255, 255, 0.9);
  padding: 2rem;
  border-radius: 20px;
  max-width: 500px;
  width: 100%;
  box-shadow: 0 12px 25px rgba(0, 0, 0, 0.2);
  backdrop-filter: blur(6px);
  margin-bottom: 2rem;
  text-align: center;
}

/* Inputs */
input, textarea {
  width: 100%;
  padding: 0.7rem;
  margin-bottom: 1rem;
  border: 2px solid #ccc;
  border-radius: 10px;
  font-size: 16px;
  outline: none;
  transition: 0.3s ease;
}

input:focus, textarea:focus {
  border-color: #6c5ce7;
}

/* Blinking button */
button {
  background: linear-gradient(90deg, #ff9a9e 0%, #fad0c4 99%, #fad0c4 100%);
  color: #fff;
  padding: 0.7rem 2rem;
  border: none;
  border-radius: 30px;
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
  box-shadow: 0 0 10px rgba(255, 105, 180, 0.6);
  animation: blink 1.5s infinite ease-in-out;
}

@keyframes blink {
  0%, 100% { opacity: 1; transform: scale(1); }
  50% { opacity: 0.7; transform: scale(1.05); }
}

/* Thanks message */
.thanks-msg {
  margin-top: 1rem;
  color: green;
  font-weight: bold;
}

/* Messages section */
.messages {
  max-width: 600px;
  background: rgba(255, 255, 255, 0.95);
  padding: 1.5rem;
  border-radius: 20px;
  box-shadow: 0 10px 20px rgba(0,0,0,0.1);
  overflow-y: auto;
  width: 100%;
}

.messages h2 {
  text-align: center;
  margin-bottom: 1rem;
}

.messages ul {
  list-style: none;
  padding: 0;
}

.messages li {
  margin-bottom: 1.5rem;
  text-align: left;
}
</style>
