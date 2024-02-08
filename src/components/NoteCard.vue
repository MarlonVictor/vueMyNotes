<script setup lang="ts">
const props = defineProps({
    historic: { type: String, required: true },
})
</script>

<template>
    <button class="text-left rounded-md bg-slate-800 p-5 space-y-3 overflow-hidden relative outline-none hover:ring-2 hover:ring-slate-600 focus-visible:ring-2 focus-visible:ring-lime-400">
        <span class="text-sm font-medium text-slate-300">{{ props.historic }}</span>
        <p class="text-sm leading-6 text-slate-400 h-full">
            <slot />
        </p>

        <div class="absolute bottom-0 left-0 right-0 h-1/2 bg-gradient-to-t from-black/60 to-black/0 pointer-events-none" />
    </button>
</template>

<style scoped></style>