<template>
    <input
        class="w-80 border-b-2 border-gray-500 text-black outline-none focus:border-green-500 transition-all duration-200"
        :class="{ 'border-red-500': error }" placeholder="Enter your task here" :value="modelValue" @input="handleInput" />
</template>

<script setup lang="ts">

interface Props {
    modelValue: string
    error?: boolean
}
interface Emits {
    (e: 'update:modelValue', value: string): void
}

defineProps<Props>()
const emits = defineEmits<Emits>()

const handleInput = (e: Event) => {
    const target = e.target as HTMLInputElement
    emits('update:modelValue', target.value)
}
</script>

<style scoped></style>