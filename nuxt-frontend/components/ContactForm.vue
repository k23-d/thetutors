<template>
    <form class="space-y-4" @submit.prevent="submitForm">
      <input v-model="name" placeholder="Your Name" class="w-full p-3 border rounded" required />
      <input v-model="email" placeholder="Your Email" type="email" class="w-full p-3 border rounded" required />
      <textarea v-model="message" placeholder="Your Message" class="w-full p-3 border rounded" rows="5" required></textarea>
      <button type="submit" class="bg-blue-600 text-white px-6 py-3 rounded hover:bg-blue-700">
        Send Message
      </button>
    </form>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  const name = ref('')
  const email = ref('')
  const message = ref('')
  
  const submitForm = () => {
    console.log('Contact Form:', { name: name.value, email: email.value, message: message.value })
    name.value = email.value = message.value = ''
  }
  </script>