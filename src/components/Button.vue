
<template>
    <button class="border-2 p-2 rounded-lg flex transition-all duration-200 active:scale-95" :class="btnStyles[btnStyle]">
        <slot>
            <svg class="h-6 w-6" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"
                fill="none" stroke-linecap="round" stroke-linejoin="round">
                <path stroke="none" d="M0 0h24v24H0z" />
                <circle cx="12" cy="12" r="9" />
                <line x1="9" y1="12" x2="15" y2="12" />
                <line x1="12" y1="9" x2="12" y2="15" />
            </svg>

            <span>Add</span>
        </slot>
    </button>
</template>

<script setup lang="ts">
interface Props {
    btnStyle: 'add-todo' | 'delete-todo' | 'reset-todo'
}

withDefaults(defineProps<Props>(), {
    btnStyle: 'add-todo'
})

const btnStyles = {
    'add-todo': "hover:bg-green-500 hover:text-white hover:bg-green-500 text-green-500 border-green-500",
    'delete-todo': "text-red-500 border-red-500",
    'reset-todo': "rounded-none !inline-block border-indigo-500 text-indigo-500"
}
</script>