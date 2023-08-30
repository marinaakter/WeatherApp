<template>
  <div class="flex flex-col flex-1 items-center">
    <!-- Banner -->
    <div
      v-if="route.query.preview"
      class="text-white p-4 bg-weather-secondary w-full text-center"
    >
      <p>
        You are currently previewing this city, click the "+" icon to start
        tracking this city.
      </p>
    </div>
    <!-- Weather Overview -->
    <div class="flex flex-col items-center text-white py-12">
      <h1 class="text-4xl mb-2">{{ route.params.city }}</h1>
      <p class="text-sm mb-12">
        {{
          new Date(weatherData.currentTime).toLocaleDateString("en-US", {
            weekday: "short",
            day: "2-digit",
            month: "long",
          })
        }}
        {{
          new Date(weatherData.currentTime).toLocaleTimeString("en-US", {
            timeStyle: "short",
          })
        }}
      </p>
      <p class="text-8xl mb-8">
        {{ farenheightToCelcius(Math.round(weatherData.current.temp)) }}&deg;
      </p>

      <p>
        Feels like
        {{
          farenheightToCelcius(Math.round(weatherData.current.feels_like))
        }}&deg;
      </p>
      <p class="capitalize">
        {{ weatherData.current.weather[0].description }}
      </p>
      <img
        class="w-[150px] h-auto"
        :src="`http://openweathermap.org/img/wn/${weatherData.current.weather[0].icon}@2x.png`"
      />
    </div>

    <hr class="border-white border-opacity-10 border w-full" />

    <!-- Hourly Weather -->
    <div class="max-w-screen-md w-full py-12">
      <div class="mx-8 text-white">
        <h2 class="mb-4">Hourly Weather</h2>
        <div class="flex gap-10 overflow-x-scroll">
          <div
            v-for="hourData in weatherData.hourly"
            :key="hourData.dt"
            class="flex flex-col gap-4 items-center"
          >
            <p class="whitespace-nowrap text-md">
              {{
                new Date(hourData.currentTime).toLocaleTimeString("en-US", {
                  hour: "numeric",
                })
              }}
            </p>
            <img
              class="w-auto h-[50px] object-cover"
              :src="`http://openweathermap.org/img/wn/${hourData.weather[0].icon}@2x.png`"
            />
            <p class="text-xl">
              {{ farenheightToCelcius(Math.round(hourData.temp)) }}&deg;
            </p>
          </div>
        </div>
      </div>
    </div>

    <hr class="border-white border-opacity-10 border w-full" />

    <!-- weekly Weather -->
    <div class="max-w-screen-md w-full py-12">
      <div class="mx-8 text-white">
        <h2 class="mb-4">7 Day Forecast</h2>
        <div
          v-for="day in weatherData.daily"
          :key="day.dt"
          class="flex items-center"
        >
          <p class="flex-1">
            {{
              new Date(day.dt * 1000).toLocaleDateString("en-US", {
                weekday: "long",
              })
            }}
          </p>
          <img
            class="w-auto h-[50px] object-cover"
            :src="`http://openweathermap.org/img/wn/${day.weather[0].icon}@2x.png`"
          />
          <div class="flex gap-2 flex-1 justify-end">
            <p>H: {{ farenheightToCelcius(Math.round(day.temp.max)) }}&deg;</p>
            <p>L: {{ farenheightToCelcius(Math.round(day.temp.min)) }}&deg;</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { useRoute } from "vue-router";

const route = useRoute();
const getWeatherData = async () => {
  try {
    const weatherData = await axios.get(
      `https://api.openweathermap.org/data/3.0/onecall?lat=33.44&lon=-94.04&appid=3407e257b5223150275037a1ed89c510&units=imperial`
    );

    //cal current data & time
    const localoffset = new Date().getTimezoneOffset() * 60000;
    const utc = weatherData.data.current.dt * 1000 + localoffset;
    weatherData.data.currentTime =
      utc + 1000 * weatherData.data.timezone_offset;

    //cal hourly weather offset
    weatherData.data.hourly.forEach((hour) => {
      const utc = hour.dt * 1000 + localoffset;
      hour.currentTime = utc + 1000 * weatherData.data.timezone_offset;
    });

    return weatherData.data;
  } catch (err) {
    console.log(err);
  }
};

const weatherData = await getWeatherData();
const farenheightToCelcius = (temp) => {
  return Math.round(((temp - 32) * 5) / 9);
};
</script>
