<script setup lang="ts">
import logo from './assets/logo-nlw.svg'
import NewNoteCard from './components/NewNoteCard.vue'
import NoteCard from './components/NoteCard.vue'
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

    <div class="grid grid-cols-3 auto-rows-[250px] gap-6">
      <NewNoteCard />

      <NoteCard 
        :note="{
          date: new Date,
          content: 'O Drizzle possui um plugin do ESLint para evitar que realizemos updates ou deletes sem where...'
        }" 
      />

      <!-- <NoteCard 
        :note="{
          date: new Date,
          content: 'No app do NLW vamos criar um layout incrível, assim podemos entregar a melhor experiência para a comunidade.'
        }" 
      /> -->
    </div>
  </div>
</template>

<style scoped>
</style>
