<template>
  <div class="w-full h-screen bg-gray-100 pt-8">

    <form @submit.prevent="addTodo" class="bg-white p-3 max-w-md mx-auto relative">
      <div v-if="isLoading" class="absolute right-1 top-1">
        <div class="spinner"></div>
      </div>
      <div class="text-center">
        <Input type="text" class="text-3xl text-center font-bold outline-none border-none" v-model="title"
          placeholder="Title" />
        <div class="mt-4 flex">
          <Input v-model="state.todo" type="text" :error="$v.todo.$error" />
          <Button class="ml-2" type="submit" />
        </div>
      </div>
      <div class="mt-8">
        <ul v-if="todoList.length > 0">
          <TodoList v-for="item of todoList" v-bind="{ ...item }" :key="item.id" @delete="deleteTodo" @toggle="toggleDone"
            @save="saveEditedTodo" @loading="isLoading = true" />
        </ul>
        <p v-else class="text-center">No Todos</p>
      </div>
      <div class="mt-8 space-x-2">
        <Button type="button" @click="resetTodoList" btn-style="reset-todo">
          Reset Todo List
        </Button>
      </div>
    </form>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, watch } from 'vue';
import { required } from '@vuelidate/validators'
import { useVuelidate } from '@vuelidate/core'
import { debounce } from './utils/index'

import Button from './components/Button.vue';
import Input from './components/From/Input.vue'
import TodoList from './components/TodoList.vue';
import { ITodo } from './types/index'

const todoList = ref([] as ITodo[])
// const todosListDone = ref([] as ITodo[])
const title = ref('ToDo App')
const isLoading = ref(false)
const state = reactive({
  todo: '',
})

const rules = {
  todo: { required },
}

const $v = useVuelidate(rules, state)

if (sessionStorage.getItem('todoList') !== null) {
  todoList.value = JSON.parse(sessionStorage.getItem('todoList') as string)
}

if (sessionStorage.getItem('title') !== null) {
  title.value = sessionStorage.getItem('title') as string
}

const createTodo = (todo: string) => {
  return {
    id: Math.random(),
    todo,
    isDone: false
  }
}

const saveToStorage = () => {
  sessionStorage.setItem('todoList', JSON.stringify(todoList.value))
}

const resetInput = () => {
  state.todo = ""
  $v.value.$reset()
}

const addTodo = () => {
  $v.value.$touch()
  if (!$v.value.todo.$error) {
    const newTodo = createTodo(state.todo)
    todoList.value.push(newTodo)
    resetInput()
    saveToStorage()
  }
}

const deleteTodo = (todoID: number) => {
  todoList.value = todoList.value.filter((el) => el.id !== todoID)
  saveToStorage()
}

const toggleDone = (todoID: number) => {
  todoList.value.forEach(el => {
    if (el.id === todoID) {
      el.isDone = !el.isDone
    }
  })

  todosListDone.value = todoList.value.filter((el) => el.isDone)
  saveToStorage()
}

const resetTodoList = () => {
  todoList.value = []
  saveToStorage()
}

// Todo
// const showOnlyDoneTodos = () => {
//   todoList.value = todosListDone.value
//   console.log(todoList.value);
// }

const saveEditedTodo = (todoID: number, text: string) => {
  todoList.value.forEach(el => {
    if (el.id === todoID) {
      el.todo = text
    }
  })
  isLoading.value = false
  alert('Saved')
  saveToStorage()
}

watch(() => title.value, (val) => {
  isLoading.value = true
  debounce('title', () => {
    alert('Project title saved')
    title.value = val
    sessionStorage.setItem('title', val)
    isLoading.value = false
  }, 1500)
})

</script>

<style scoped></style>