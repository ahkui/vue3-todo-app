<template>
  <h1>Hello {{ world }}</h1>
  <input type="text" v-model="world" />
</template>

<script setup>
import { ref } from "vue";

const world = ref("world");
</script>
