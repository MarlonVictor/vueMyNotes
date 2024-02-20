<script setup lang="ts">
import { formatDistanceToNow } from 'date-fns'
import { ptBR } from 'date-fns/locale'
import { X } from 'lucide-vue-next'

import {
  DialogRoot,
  DialogTrigger,
  DialogPortal,
  DialogOverlay,
  DialogContent,
  DialogClose
} from 'radix-vue'

interface NoteCardProps {
    note: {
        date: Date,
        content?: string
    }
}

const { note } = defineProps<NoteCardProps>()
const dateConfig = { locale: ptBR, addSuffix: true }
</script>

<template>
    <DialogRoot>
        <DialogTrigger class="text-left rounded-md bg-slate-800 p-5 space-y-3 overflow-hidden relative outline-none hover:ring-2 hover:ring-slate-600 focus-visible:ring-2 focus-visible:ring-lime-400">
            <span class="text-sm font-medium text-slate-300">
                {{ formatDistanceToNow(note.date, dateConfig) }}
            </span>

            <p class="text-sm leading-6 text-slate-400 h-full">
                {{ note.content }}
            </p>
    
            <div class="absolute bottom-0 left-0 right-0 h-1/2 bg-gradient-to-t from-black/60 to-black/0 pointer-events-none" />
        </DialogTrigger>

        <DialogPortal>
            <DialogOverlay class="inset-0 fixed bg-black/50" />

            <DialogContent class="fixed inset-0 md:inset-auto md:left-1/2 md:-translate-x-1/2 md:top-1/2 md:-translate-y-1/2 md:max-w-[640px] md:h-[60vh] w-full bg-slate-700 md:rounded-md flex flex-col outline-none overflow-hidden">
                <DialogClose class="absolute right-0 top-0 bg-slate-800 p-1.5 text-slate-400 rounded-bl-md hover:text-slate-100">
                    <X class="size-5" />
                </DialogClose>

                <div class="flex flex-1 flex-col gap-3 p-5">
                    <span class="text-sm font-medium text-slate-300">
                        {{ formatDistanceToNow(note.date, dateConfig) }}
                    </span>
                    
                    <p class="text-sm leading-6 text-slate-400 h-full">
                        {{ note.content }}
                    </p>
                </div>

                <button 
                    type="button"
                    class="w-full bg-slate-800 py-4 text-center text-sm text-slate-300 outline-none font-medium group"
                >
                    Deseja <span class="text-red-400 group-hover:underline">apagar essa nota</span>?
                </button>
            </DialogContent>
        </DialogPortal>
    </DialogRoot>
</template>

<style scoped></style>