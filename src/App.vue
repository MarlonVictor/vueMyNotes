<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'
import { Toaster, toast } from 'vue-sonner'

import logo from './assets/logo-nlw.svg'
import NewNoteCard from './components/NewNoteCard.vue'
import NoteCard from './components/NoteCard.vue'

interface Note {
  id: string,
  date: Date,
  content?: string
}

const notes = ref<Note[]>([])
const filteredNotes = ref<Note[]>([])
const search = ref('')

const onNoteCreated = (content: string) => {
  const newNote = { 
    id: crypto.randomUUID(), 
    date: new Date, 
    content
  }

  const notesArray = [newNote, ...notes.value]

  notes.value = notesArray
  localStorage.setItem('notes', JSON.stringify(notesArray))
}

const onNoteDeleted = (id: string) => {
  const notesArray = notes.value.filter(note => note.id !== id)

  notes.value = notesArray
  localStorage.setItem('notes', JSON.stringify(notesArray))

  toast.info('Nota deletada!')
}

watch(
  () => search.value,
  (query) => {    
    filteredNotes.value = query !== ''
      ? notes.value.filter(note => note.content?.toLocaleLowerCase().includes(query.toLocaleLowerCase()))
      : notes.value
  }
)

watch(
  () => notes.value,
  (notes) => filteredNotes.value = notes
)

onMounted(() => {
  const notesOnStorage = localStorage.getItem('notes')

  if (notesOnStorage) {
    notes.value = JSON.parse(notesOnStorage)
    filteredNotes.value = notes.value
  }
})
</script>

<template>
  <div class="max-w-6xl mx-auto my-12 space-y-6 px-5 relative">
    <img 
      :src="logo" 
      alt="NLW Expert" 
      class="xl:absolute xl:-left-28 xl:top-16 xl:-rotate-90 xl:w-40"
    />

    <form>
      <input 
        v-model="search"
        type="text" 
        placeholder="Busque em suas notas..." 
        class="bg-transparent w-full text-3xl font-semibold tracking-tight outline-none placeholder:text-slate-500"
      />
    </form>

    <div class="h-px bg-slate-700" />

    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 auto-rows-[250px] gap-6">
      <NewNoteCard :onNoteCreated="onNoteCreated" />

      <NoteCard 
        v-for="note in filteredNotes"
        :key="note.id"
        :note="note" 
        :onNoteDeleted="onNoteDeleted"
      />

      <Toaster rich-colors />
    </div>
  </div>
</template>

<style scoped>
</style>
