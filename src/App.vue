<script setup lang="ts">
import logo from './assets/logo-nlw.svg'
</script>

<template>
  <div class="max-w-6xl mx-auto my-12 space-y-6">
    <img :src="logo" alt="NLW Expert" />

    <form>
      <input 
        type="text" 
        placeholder="Busque em suas notas..." 
        class="bg-transparent w-full text-3xl font-semibold tracking-tight outline-none placeholder:text-slate-500"
      />
    </form>

    <div class="h-px bg-slate-700" />
  </div>
</template>

<style scoped>
</style>
