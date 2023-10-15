<template>
    <input type="checkbox" class="h-6 w-6 " :value="modelValue" :checked="modelValue" @change="handleChange" />
</template>

<script setup lang="ts">
interface Props {
    modelValue: boolean
}
interface Emits {
    (e: 'update:modelValue', value: boolean): void
}

defineProps<Props>()
const emits = defineEmits<Emits>()

const handleChange = (e: Event) => {
    const target = e.target as HTMLInputElement
    emits('update:modelValue', target.checked)
}
</script>

<style scoped></style>