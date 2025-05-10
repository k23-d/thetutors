<template>
    <section class="text-center py-24 bg-gradient-to-br from-blue-50 to-blue-100">
      <h1 class="text-5xl font-bold text-blue-700 animate-fade-in-up">
        Discover AI Tools Built for You
      </h1>
      <p class="mt-4 text-gray-600 max-w-xl mx-auto">
        Automate tasks, create smarter workflows, and explore what's possible with The-Tutors AI Marketplace.
      </p>
      <NuxtLink to="/explore" class="inline-block mt-6 px-6 py-3 bg-blue-600 text-white rounded-full hover:bg-blue-700">
        Explore Tools
      </NuxtLink>
    </section>
  </template>
  
  <style scoped>
  @keyframes fade-in-up {
    0% { opacity: 0; transform: translateY(20px); }
    100% { opacity: 1; transform: translateY(0); }
  }
  .animate-fade-in-up {
    animation: fade-in-up 1s ease-out;
  }
  </style>