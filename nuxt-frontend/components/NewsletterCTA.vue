<template>
    <section class="bg-white py-16 px-4 text-center">
      <h2 class="text-2xl font-semibold text-gray-800">Stay in the Loop</h2>
      <p class="text-gray-600 mb-6">Subscribe to our newsletter for updates on tools and features.</p>
      <form @submit.prevent="subscribe" class="flex justify-center gap-4 max-w-xl mx-auto">
        <input v-model="email" type="email" placeholder="Enter your email" class="p-3 border rounded w-full" required />
        <button type="submit" class="bg-blue-600 text-white px-6 py-3 rounded hover:bg-blue-700">
          Subscribe
        </button>
      </form>
    </section>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  const email = ref('')
  const subscribe = () => {
    console.log('Subscribed:', email.value)
    email.value = ''
  }
  </script>