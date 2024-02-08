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

    <div class="grid grid-cols-3 auto-rows-[250px] gap-6">

      <div class="rounded-md bg-slate-700 p-5 space-y-3 overflow-hidden relative ">
        <span class="text-sm font-medium text-slate-200">Adicionar nota</span>
        <p class="text-sm leading-6 text-slate-400">Grave uma nota em áudio que será convertida para texto automaticamente.</p>
      </div>

      <div class="rounded-md bg-slate-800 p-5 space-y-3 overflow-hidden relative ">
        <span class="text-sm font-medium text-slate-300">há 2 dias</span>
        <p class="text-sm leading-6 text-slate-400">O Drizzle possui um plugin do ESLint para evitar que realizemos updates ou deletes sem where...</p>

        <div class="absolute bottom-0 left-0 right-0 h-1/2 bg-gradient-to-t from-black/60 to-black/0 pointer-events-none" />
      </div>

      <div class="rounded-md bg-slate-800 p-5 space-y-3 overflow-hidden relative ">
        <span class="text-sm font-medium text-slate-300">há 4 dias</span>
        <p class="text-sm leading-6 text-slate-400">
          No app do NLW vamos criar um layout incrível, assim podemos entregar a melhor experiência para a comunidade. <br /> <br />
          Na aplicação React vamos criar um projeto que permite o usuário salvar notas em texto ou áudio.
        </p>

        <div class="absolute bottom-0 left-0 right-0 h-1/2 bg-gradient-to-t from-black/60 to-black/0 pointer-events-none" />
      </div>
    </div>
  </div>
</template>

<style scoped>
</style>
