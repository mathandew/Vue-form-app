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

    <p v-if="submitted">✅ Thanks {{ form.name }}, we got your message!</p>

    <div v-if="storedData">
      <h3>Stored from LocalStorage:</h3>
      <p><strong>Name:</strong> {{ storedData.name }}</p>
      <p><strong>Email:</strong> {{ storedData.email }}</p>
      <p><strong>Message:</strong> {{ storedData.message }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const form = ref({
  name: '',
  email: '',
  message: ''
})

const submitted = ref(false)
const storedData = ref(null)

const submitForm = () => {
  if (!form.value.name || !form.value.email || !form.value.message) {
    alert('All fields are required!')
    return
  }

  localStorage.setItem('formData', JSON.stringify(form.value))
  submitted.value = true
}

onMounted(() => {
  console.log('Component mounted!')
  const data = localStorage.getItem('formData')
  if (data) {
    storedData.value = JSON.parse(data)
  }
})
</script>

<style scoped>
.form-wrapper {
  max-width: 400px;
  margin: auto;
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 10px;
}
form {
  display: flex;
  flex-direction: column;
}
input, textarea {
  margin-bottom: 1rem;
  padding: 0.5rem;
}
button {
  background-color: #42b983;
  color: white;
  padding: 0.5rem;
  border: none;
  cursor: pointer;
}
</style>
