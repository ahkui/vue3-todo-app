<template>
  <ul>
    <li v-for="item in todos" :key="item.id">{{ item.text }}</li>
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
