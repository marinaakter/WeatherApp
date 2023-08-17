<template>
  <header class="sticky top-0 shadow-lg bg-weather-primary">
    <nav class="container flex flex-col items-center gap-4 py-6 text-white sm:flex-row">
      <RouterLink :to="{ name: 'home' }">
        <div class="flex items-center gap-3">
          <i class="text-2xl fa-solid fa-sun"></i>
          <p class="text-2xl">The Local Weather</p>
        </div>
      </RouterLink>

      <div class="flex justify-end flex-1 gap-3">
        <i class="text-xl duration-150 cursor-pointer fa-solid fa-circle-info hover:text-weather-secondary"
          @click="toggleModel"></i>
        <i class="text-xl duration-150 cursor-pointer fa-solid fa-plus hover:text-weather-secondary"></i>
      </div>

      <BaseModel :modelActive="modelActive" @Close-modal="toggleModel">
        <div class="text-black">
          <h1 class="mb-1 text-2xl">About:</h1>
          <p class="mb-4 ">
            The Local weather allows you to track current and future weather of cities of your choosing.
          </p>
          <h2 class="text-2xl ">How it Works:</h2>
          <ol class="mb-4 list-decimal list-inside ">
            <li>
              Search for a city by entering the name into the search bar.
            </li>
            <li>
              Select a city within the results, this will take you to the current weather to your selection.
            </li>
            <li>
              Track the city by clicking on the '+' icon in the top right. This will save the city to view at a later time
              on the Home page.
            </li>
          </ol>
          <h2 class="text-2xl ">Removing a City:</h2>
          <p> If you no longer wish to track a city, simply select the city within the home page. At the bottom of the
            page, there will be an option to delete the city</p>
        </div>



      </BaseModel>
    </nav>
  </header>
</template>


<script setup>
import { ref } from 'vue';
import { RouterLink } from 'vue-router';
import BaseModel from './BaseModel.vue';

const modelActive = ref(null);
const toggleModel = () => {
  modelActive.value = !modelActive.value;
};


</script>
