<template>
  <div v-for="city in savedCities" :key="city.id">
    <CityCard :city="city" />
  </div>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";
import CityCard from "./CityCard.vue";

const savedCities = ref([]);
const getCities = async () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(localStorage.getItem("savedCities"));

    const requests = [];
    savedCities.value.forEach((city) => {
      requests.push(
        axios.get(
          `https://api.openweathermap.org/data/2.5/weather?
          lat=44.34&lon=10.99&appid=3407e257b5223150275037a1ed89c510&units=imperial`
        )
      );
    });

    const weatherData = await Promise.all(requests);

    weatherData.forEach((data, index) => {
      savedCities.value[index].weather = value.data;
    });
  }
};

await getCities();
</script>
