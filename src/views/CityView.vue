<template>
    <div>
        <Suspense>
            <AsyncCityView/>
            <template #fallback >
                <p class="text-white py-5 px-10">Loading...</p>
            </template>
        </Suspense>
    </div>
</template>

<script setup>
import AsyncCityView from "../components/AsyncCityView.vue";
</script>
