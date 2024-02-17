<script setup lang="ts">
import { ref } from 'vue'
import { X } from 'lucide-vue-next'
import { toast } from 'vue-sonner'

import { 
  DialogClose, 
  DialogContent, 
  DialogOverlay, 
  DialogPortal, 
  DialogRoot, 
  DialogTrigger 
} from 'radix-vue'

interface NewNoteCardProps {
    onNoteCreated: (content: string) => void
}

const { onNoteCreated } = defineProps<NewNoteCardProps>()

const shouldShowOnboarding = ref(true)
const content = ref<string>()

const handleStartEditor = () => {
    shouldShowOnboarding.value = false
}

const handleContentChanged = (event: any) => {
    const { value: textareaValue } = event.target
    content.value = textareaValue

    if (textareaValue === '') {
        shouldShowOnboarding.value = true
    }
}

const handleSaveNote = (event: any) => {
    event.preventDefault()

    if (!content.value) {
        toast.error('Digite algo na nota!')
        return
    }

    onNoteCreated(content.value || '')
    toast.success('Nota criada com sucesso!')

    content.value = ''
    shouldShowOnboarding.value = true
}
</script>

<template>
    <DialogRoot>
        <DialogTrigger class="rounded-md bg-slate-700 text-left p-5 gap-y-3 overflow-hidden relative flex flex-col outline-none hover:ring-2 hover:ring-slate-600 focus-visible:ring-2 focus-visible:ring-lime-400">
            <span class="text-sm font-medium text-slate-200">
                Adicionar nota
            </span>

            <p class="text-sm leading-6 text-slate-400">
                Grave uma nota em áudio que será convertida para texto automaticamente.
            </p>
        </DialogTrigger>

        <DialogPortal>
            <DialogOverlay class="inset-0 fixed bg-black/50" />

            <DialogContent class="fixed left-1/2 -translate-x-1/2 top-1/2 -translate-y-1/2 max-w-[640px] h-[60vh] w-full bg-slate-700 rounded-md flex flex-col outline-none overflow-hidden">
                <DialogClose class="absolute right-0 top-0 bg-slate-800 p-1.5 text-slate-400 rounded-bl-md hover:text-slate-100">
                    <X class="size-5" />
                </DialogClose>

                <form @submit="handleSaveNote" class="flex flex-col flex-1">
                    <div class="flex flex-1 flex-col gap-3 p-5">
                        <span class="text-sm font-medium text-slate-300">
                            Adicionar nota
                        </span>
                        
                        <p v-if="shouldShowOnboarding" class="text-sm leading-6 text-slate-400 h-full">
                            Comece 
                            <button class="font-medium text-lime-400 hover:underline">
                                gravando uma nota
                            </button> 
                            em áudio ou se preferir 
                            <button @click="handleStartEditor" class="font-medium text-lime-400 hover:underline">
                                utilize apenas texto
                            </button>.
                        </p>
    
                        <textarea
                            v-else 
                            autofocus 
                            placeholder="Digite aqui..."
                            class="text-sm leading-6 text-slate-400 bg-transparent resize-none flex-1 outline-none"
                            :value="content"
                            @input="handleContentChanged"
                        />
                    </div>
    
                    <button 
                        type="submit"
                        class="w-full bg-lime-400 py-4 text-center text-sm text-lime-950 outline-none font-medium hover:bg-lime-500"
                    >
                        Salvar nota
                    </button>
                </form>
            </DialogContent>
        </DialogPortal>
    </DialogRoot>
</template>

<style scoped></style>