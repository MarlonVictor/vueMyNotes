<script setup lang="ts">
import logo from './assets/logo-nlw.svg'
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

      <div class="rounded-md bg-slate-700 p-5 space-y-3 overflow-hidden relative ">
        <span class="text-sm font-medium text-slate-200">Adicionar nota</span>
        <p class="text-sm leading-6 text-slate-400">Grave uma nota em áudio que será convertida para texto automaticamente.</p>
      </div>

      <NoteCard historic="há 2 dias">
        O Drizzle possui um plugin do ESLint para evitar que realizemos updates ou deletes sem where... <br /> <br />
        Na aplicação React vamos criar um projeto que permite o usuário salvar notas em texto ou áudio.
      </NoteCard>

      <NoteCard historic="há 4 dias">
        No app do NLW vamos criar um layout incrível, assim podemos entregar a melhor experiência para a comunidade. 
      </NoteCard>
    </div>
  </div>
</template>

<style scoped>
</style>
