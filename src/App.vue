<script setup>
import { RouterLink, RouterView } from "vue-router";
import MainHeader from "./components/MainHeader.vue";
</script>

<template>
  <header class="w-full bg-gray-800 text-white">
    <div class="mx-auto">
      <MainHeader />
      <nav class="text-center py-5 bg-gray-700 rounded">
        <RouterLink class="mx-2 text-xs" to="/">Home</RouterLink>
        <RouterLink class="mx-2 text-xs" to="/project">Projeto 📃</RouterLink>
      </nav>
    </div>
  </header>
  <RouterView />
</template>
