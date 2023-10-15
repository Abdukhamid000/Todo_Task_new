<template>
  <div class="w-full h-screen bg-gray-100 pt-8">
    <form @submit.prevent="addTodo" class="bg-white p-3 max-w-md mx-auto">
      <div class="text-center">
        <h1 class="text-3xl font-bold">ToDo App</h1>
        <div class="mt-4 flex">
          <Input v-model="state.todo" type="text" :error="$v.todo.$error" />
          <Button class="ml-2" type="submit" />
        </div>
      </div>
      <div class="mt-8">
        <ul v-if="todoList.length > 0">
          <TodoList v-for="item of todoList" v-bind="{ ...item }" :key="item.id" @delete="deleteTodo"
            @toggle="toggleDone" />
        </ul>
        <p v-else class="text-center">No Todos</p>
      </div>
      <div class="mt-8 space-x-2">
        <Button class="rounded-none !inline-block">
          Clear Completed Task
        </Button>
        <Button @click="resetTodoList" btn-style="reset-todo">
          Reset Todo List
        </Button>
      </div>
    </form>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue';
import { required } from '@vuelidate/validators'
import { useVuelidate } from '@vuelidate/core'

import Button from './components/Button.vue';
import Input from './components/From/Input.vue'
import TodoList from './components/TodoList.vue';

interface ITodo {
  id: number,
  todo: string,
  isDone: boolean
}

const todoList = ref([] as ITodo[])

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

  saveToStorage()
}

const resetTodoList = () => {
  todoList.value = []
  saveToStorage()
}

</script>

<style scoped></style>