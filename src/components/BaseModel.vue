<template>
    <Transition>
        <div v-show="modelActive"
            class="absolute top-0 left-0 flex justify-center w-full h-screen px-8 bg-black bg-opacity-30">
            <Transition>
                <div v-if="modelActive" class="self-start max-w-screen-md p-4 mt-32 bg-white rounded-lg">
                    <slot />
                    <button class="px-6 py-2 mt-8 text-white bg-gray-800 rounded-md"
                        @click="$emit('Close-modal')">Close</button>
                </div>
            </Transition>
        </div>
    </Transition>
</template>

<script setup>
defineEmits(["Close-modal"])
defineProps({
    modelActive: {
        type: Boolean,
        default: false,
    }
})
</script>   

<style lang="scss" scoped></style>