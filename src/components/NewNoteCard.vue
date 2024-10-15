<script setup lang="ts">
import { ArrowUpRight, X } from 'lucide-vue-next';
import { onMounted, ref } from 'vue';
import { toast } from 'vue-sonner';

import {
    DialogClose,
    DialogContent,
    DialogOverlay,
    DialogPortal,
    DialogRoot,
    DialogTrigger
} from 'radix-vue';

interface NewNoteCardProps {
    onNoteCreated: (content: string) => void
}

const { onNoteCreated } = defineProps<NewNoteCardProps>()

const shouldShowOnboarding = ref(true)
const isRecording = ref(false)
const content = ref<string>()

const openDialog = ref(false)

let speechRecognition: SpeechRecognition | null = null

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

const handleSaveNote = () => {
    if (!content.value) {
        toast.error('Digite algo na nota!')
        return
    }

    onNoteCreated(content.value || '')
    toast.success('Nota criada com sucesso!')

    content.value = ''
    shouldShowOnboarding.value = true
    openDialog.value = false
}

const handleStartRecording = () => {
    isRecording.value = true

    const isSpeechRecognitionAPIAvailable = 'SpeechRecognition' in window
        || 'webkitSpeechRecognition' in window

    if (!isSpeechRecognitionAPIAvailable) {
        toast.warning('Infelizmente seu navegador não suporta a API de gravação!')
        isRecording.value = false
        return
    }

    const SpeechRecognitionAPI = window.SpeechRecognition || window.webkitSpeechRecognition
    speechRecognition = new SpeechRecognitionAPI()

    speechRecognition.lang = 'pt-BR'
    speechRecognition.continuous = true
    speechRecognition.maxAlternatives = 1
    speechRecognition.interimResults = true

    speechRecognition.onresult = (event) => {
        const transcription = Array.from(event.results).reduce((text, result) => {
            return text.concat(result[0].transcript)
        }, '')        

        content.value = transcription
    }

    speechRecognition.onerror = (event) => {
        console.error(event)
        toast.error('Ocorreu algum erro ao gravar')
    }

    speechRecognition.start()
}

const handleStopRecording = () => {
    isRecording.value = false
    speechRecognition?.stop()
}

onMounted(() => {
    document.addEventListener('keydown', (ev) => {
        if (ev.ctrlKey &&  ev.key === 'q') {
            ev.preventDefault()
            handleSaveNote()
        }
    });
})
</script>

<template>
    <DialogRoot v-model:open="openDialog">
        <div class="rounded-md bg-slate-700 text-left overflow-hidden relative flex flex-col outline-none focus-visible:ring-2 focus-visible:ring-lime-400">
            <form class="flex flex-col flex-1">
                <DialogTrigger
                    type="button"
                    class="bg-slate-800 w-max p-1.5 absolute top-0 right-0 text-slate-300 hover:text-slate-100 rounded-bl-md"
                >
                    <ArrowUpRight class="size-5" />
                </DialogTrigger>

                <div class="flex flex-col gap-y-3 p-5 flex-1">
                    <span class="text-sm font-medium text-slate-200">
                        Adicionar nota
                    </span>
        
                    <p 
                        v-if="shouldShowOnboarding" 
                        @click="handleStartEditor" 
                        class="text-sm leading-6 text-slate-400 flex-1"
                    >
                        Escreva seus pensamentos ou grave uma 
                        <span class="underline cursor-pointer" @click="handleStartRecording">nota em áudio</span>.
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
                    v-if="isRecording"
                    type="button"
                    class="flex items-center justify-center gap-2 w-full bg-slate-900 py-4 text-center text-sm text-slate-300 outline-none font-medium border-b border-x border-slate-800 rounded-b-md hover:text-slate-100"
                    @click="handleStopRecording"
                >
                    <span class="size-3 rounded-full bg-red-500 animate-pulse" />
                    Gravando! (clique p/ interromper)
                </button>

                <span v-else class="w-full bg-slate-800 py-4 text-center text-sm text-slate-300 outline-none font-medium cursor-default">
                    Aperte CMD + Q p/ salvar
                </span>
            </form>
        </div>

        <DialogPortal>
            <DialogOverlay class="inset-0 fixed bg-black/50" />

            <DialogContent class="fixed inset-0 md:inset-auto md:left-1/2 md:-translate-x-1/2 md:top-1/2 md:-translate-y-1/2 md:max-w-[640px] md:h-[60vh] w-full bg-slate-700 md:rounded-md flex flex-col outline-none overflow-hidden">
                <DialogClose class="absolute right-0 top-0 bg-slate-800 p-1.5 text-slate-400 rounded-bl-md hover:text-slate-100">
                    <X class="size-5" />
                </DialogClose>

                <form class="flex flex-col flex-1">
                    <div class="flex flex-1 flex-col gap-3 p-5">
                        <span class="text-sm font-medium text-slate-300">
                            Adicionar nota
                        </span>
                        
                        <p v-if="shouldShowOnboarding" class="text-sm leading-6 text-slate-400 h-full">
                            Comece 
                            <button 
                                type="button" 
                                @click="handleStartRecording" 
                                class="font-medium text-lime-400 hover:underline"
                            >
                                gravando uma nota
                            </button> 

                            em áudio ou se preferir 
                            <button 
                                type="button" 
                                @click="handleStartEditor" 
                                class="font-medium text-lime-400 hover:underline"
                            >
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
                        v-if="isRecording"
                        type="button"
                        class="flex items-center justify-center gap-2 w-full bg-slate-900 py-4 text-center text-sm text-slate-300 outline-none font-medium hover:text-slate-100"
                        @click="handleStopRecording"
                    >
                        <span class="size-3 rounded-full bg-red-500 animate-pulse" />
                        Gravando! (clique p/ interromper)
                    </button>
                    <button 
                        v-else
                        type="button"
                        @click="handleSaveNote"
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