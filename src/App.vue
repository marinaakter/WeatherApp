<template>
  <div class="flex flex-col min-h-screen bg-gray-700 font-Roboto">
    <siteNavigation />
    <RouterView />
  </div>
</template>

<script setup>
import { RouterView } from 'vue-router'
import siteNavigation from './components/siteNavigation.vue'
</script>

<style lang="scss" scoped></style>