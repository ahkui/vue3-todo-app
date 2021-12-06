<template>
  <ul>
    <li>{{ todos[0].text }}</li>
    <li>{{ todos[1].text }}</li>
    <li>{{ todos[2].text }}</li>
  </ul>
</template>

<script setup>
import { ref } from "vue";

const todos = ref([
  { id: 1, text: "Learn Vue 3" },
  { id: 2, text: "Create Todo App" },
  { id: 3, text: "Feedback" },
]);
</script>
