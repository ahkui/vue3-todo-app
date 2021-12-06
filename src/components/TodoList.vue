<template>
  <input type="text" v-model="todoInput" />
  <button @click="addTodo">Add</button>

  <ul>
    <li v-for="item in todos" :key="item.id">{{ item.text }}</li>
  </ul>
</template>

<script setup>
import { ref } from "vue";

const nextId = ref(4);
const todos = ref([
  { id: 1, text: "Learn Vue 3" },
  { id: 2, text: "Create Todo App" },
  { id: 3, text: "Feedback" },
]);

const todoInput = ref("");
function addTodo() {
  todos.value.push({ id: nextId.value, text: todoInput.value });
  nextId.value++;
  todoInput.value = "";
}
</script>
