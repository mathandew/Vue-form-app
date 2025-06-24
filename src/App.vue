<template>
  <div class="form-wrapper">
    <h1>Contact Form</h1>
    <form @submit.prevent="submitForm">
      <label>Name:</label>
      <input type="text" v-model="form.name" />

      <label>Email:</label>
      <input type="email" v-model="form.email" />

      <label>Message:</label>
      <textarea v-model="form.message"></textarea>

      <button type="submit">Submit</button>
    </form>

    <p v-if="submitted" class="thanks-msg">
      ✅ Thanks {{ lastSubmittedName }}, we got your message!
    </p>

    <div v-if="storedMessages.length">
      <h3>All Submitted Messages:</h3>
      <ul>
        <li v-for="(msg, index) in storedMessages" :key="index">
          <p><strong>{{ msg.name }}</strong> ({{ msg.email }}) — {{ msg.submittedAt }}</p>
          <p>{{ msg.message }}</p>
          <hr />
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

// form data
const form = ref({
  name: '',
  email: '',
  message: ''
})

// status flags
const submitted = ref(false)
const lastSubmittedName = ref('')

// stored data
const storedMessages = ref([])

const submitForm = () => {
  if (!form.value.name || !form.value.email || !form.value.message) {
    alert('All fields are required!')
    return
  }

  const newMessage = {
    ...form.value,
    submittedAt: new Date().toLocaleString()
  }

  // Get existing messages
  const messages = JSON.parse(localStorage.getItem('formMessages')) || []

  // Add new message
  messages.push(newMessage)

  // Save updated list to localStorage
  localStorage.setItem('formMessages', JSON.stringify(messages))

  // Reset form
  lastSubmittedName.value = form.value.name
  form.value.name = ''
  form.value.email = ''
  form.value.message = ''
  submitted.value = true

  // Update display
  storedMessages.value = messages
}

onMounted(() => {
  const messages = localStorage.getItem('formMessages')
  if (messages) {
    storedMessages.value = JSON.parse(messages)
  }
})
</script>

<style scoped>
.form-wrapper {
  max-width: 500px;
  margin: auto;
  padding: 1.5rem;
  border: 1px solid #ccc;
  border-radius: 10px;
  background: #f9f9f9;
}
form {
  display: flex;
  flex-direction: column;
}
input, textarea {
  margin-bottom: 1rem;
  padding: 0.5rem;
  font-size: 16px;
}
button {
  background-color: #42b983;
  color: white;
  padding: 0.5rem;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}
.thanks-msg {
  margin-top: 1rem;
  font-weight: bold;
  color: green;
}
ul {
  list-style: none;
  padding: 0;
}
hr {
  margin: 1rem 0;
}
</style>
