<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <h1>这是主应用</h1>
  <!-- 子应用 -->
  <wujieVue url="http://localhost:5174/" name="react"></WujieVue>
  <wujieVue url="http://localhost:5175/" name="vue3"></WujieVue>
</template>

<style scoped>
</style>
