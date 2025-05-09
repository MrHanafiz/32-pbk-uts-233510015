<template>
<div class="app-container">
    <h1>My To-Do-List</h1>
    <form @submit.prevent="addTodo" class="todo-form">
      <input v-model="newTodo" placeholder="Bos, silahkan isi kegiatan anda" />
      <button type="submit">Tambah</button>
    </form>
    <div class="filter-buttons">
      <button :class="{ active: filter === 'all' }" @click="filter = 'all'">Semua</button>
      <button :class="{ active: filter === 'active' }" @click="filter = 'active'">Belum Selesai</button>
      <button :class="{ active: filter === 'done' }" @click="filter = 'done'">Sudah Selesai</button>
    </div>
    <ul class="todo-list">
      <li v-for="(todo, index) in filteredTodos" :key="index" class="todo-item">
        <label class="checkbox-label">
          <input type="checkbox" v-model="todo.done" />
          <span :class="{ done: todo.done }">{{ todo.text }}</span>
        <button class="delete-button" @click="removeTodo(index)">🗑</button>
      </li>
    </ul>
  </div>

</template>

<script setup>
import { ref, computed } from 'vue'

const todos = ref([])
const newTodo = ref('')
const filter = ref('all')

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({ text: newTodo.value, done: false })
    newTodo.value = ''
  }
}

const removeTodo = (index) => {
  todos.value.splice(index, 1)
}

const filteredTodos = computed(() => {
  if (filter.value === 'active') {
    return todos.value.filter(todo => !todo.done)
  } else if (filter.value === 'done') {
    return todos.value.filter(todo => todo.done)
  }
  return todos.value
})
</script>


