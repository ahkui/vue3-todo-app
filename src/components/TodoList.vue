<template>
  <input type="text" v-model="todoInput" />
  <button @click="addTodo">Add</button>

  <p>Total todos: {{ todos.length }}</p>
  <p>Total todo complete: {{ completedTodos.length }}</p>
  <p>Total todo active: {{ activeTodos.length }}</p>

  <ul>
    <todo-item
      v-for="item in todos"
      :key="item.id"
      :item="item"
      @todo:delete="deleteTodo"
    ></todo-item>
  </ul>
</template>

<script setup>
import { ref, computed } from "vue";

import TodoItem from "./TodoItem.vue";

const nextId = ref(4);
const todos = ref([
  { id: 1, done: false, text: "Learn Vue 3" },
  { id: 2, done: false, text: "Create Todo App" },
  { id: 3, done: false, text: "Feedback" },
]);

const activeTodos = computed(() => todos.value.filter((todo) => !todo.done));
const completedTodos = computed(() => todos.value.filter((todo) => todo.done));

const todoInput = ref("");
function addTodo() {
  todos.value.push({ id: nextId.value, text: todoInput.value });
  nextId.value++;
  todoInput.value = "";
}

function deleteTodo(item) {
  todos.value = todos.value.filter((todo) => todo.id != item.id);
}
</script>
