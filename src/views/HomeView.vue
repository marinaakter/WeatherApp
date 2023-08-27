<template>
  <main class="container text-white">
    <div class="relative pt-4 mb-8">
      <input
        type="text"
        v-model="searchQuery"
        @input="getSearchResults"
        placeholder="Search For a City or State"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      />
      <ul
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
        v-if="mapboxSearchResults"
      >
      <p v-if="searchError" >Sorry, something went wrong, please try again.</p>
      <p v-if="!serverError && mapboxSearchResults.length===0" > No results match you query, try a different term.</p>
        <template v-else >
          <li
            v-for="searchResult in mapboxSearchResults"
            :key="searchResult.id"
            class="py-2 cursor-pointer"
            @click="previewCity(searchResult)"
          >
            {{ searchResult.place_name }}
          </li>
        </template>
      </ul>
    </div>
  </main>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const previewCity = (searchResult) => {
  console.log(searchResult);
  const [city, state] = searchResult.place_name.split(", ");
  router.push({
    name: "cityView",
    params: {
      state:state.replaceAll(" ",""),city:city,
      },
    query: {
      lat: searchResult.geometry.coordinates[1],
      lng: searchResult.geometry.coordinates[0],
      previw: true,
    }
  });
};

const mapboxAPIKEY =
  "pk.eyJ1IjoibmFzaW11bGhhc2FuIiwiYSI6ImNrYWR2N3FnYjBibDQyeG5sdmhnaDZkc2wifQ.3Us9I385DLrGnq9o8-et-A";

const searchQuery = ref("");
const queryTimeOut = ref(null);
const mapboxSearchResults = ref(null);
const searchError = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeOut.value);
  queryTimeOut.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      try {
         const result = await axios.get(
        `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKEY}`
      );
      mapboxSearchResults.value = result.data.features;
      } catch {
        searchError.value = true;
      }

      return;
    }
    mapboxSearchResults.value = null;
  }, 300);
};
</script>
