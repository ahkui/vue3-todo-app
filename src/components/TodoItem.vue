<template>
  <li>{{ item.text }}</li>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps(['item']);

</script>
