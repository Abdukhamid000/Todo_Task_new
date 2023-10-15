<template>
    <div>
        <li class="p-2 rounded-lg overflow-auto">
            <div class="flex align-middle flex-row justify-between">
                <div class="p-2">
                    <InputCheckbox type="checkbox" :model-value="isDone" @update:model-value="change" />
                </div>
                <div class="p-2">

                    <Input :disabled="isDone" :class="{ 'line-through': isDone }"
                        class="text-lg text-gray-400 w-full border-none text-center outline-none" v-model="todoText" />

                </div>
                <Button @click="$emit('delete', id)" btn-style="delete-todo">
                    <svg class="h-6 w-6 text-red-500" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"
                        stroke-linecap="round" stroke-linejoin="round">
                        <circle cx="12" cy="12" r="10" />
                        <line x1="15" y1="9" x2="9" y2="15" />
                        <line x1="9" y1="9" x2="15" y2="15" />
                    </svg>
                    <span>Remove</span>
                </button>
            </div>
            <hr class="mt-2" />
        </li>
    </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';
import { debounce } from '../utils';

import Button from './Button.vue';
import InputCheckbox from './From/InputCheckbox.vue';
import Input from './From/Input.vue';
import { ITodo } from '../types';

interface Props extends ITodo { }

interface Emits {
    (e: 'delete', id: number): void
    (e: 'toggle', id: number): void
    (e: 'save', id: number, val: string): void
    (e: 'loading'): void
}

const props = defineProps<Props>()
const emits = defineEmits<Emits>()

const todoText = ref(props.todo)

const change = () => {
    emits('toggle', props.id)
}

watch(() => todoText.value, (val) => {
    emits('loading')
    debounce('todoText', () => {
        emits('save', props.id, val)
    }, 1500)
})
</script>

<style scoped></style>