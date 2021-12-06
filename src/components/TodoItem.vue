<template>
  <li>
    <button @click="complete">完成</button>
    <button @click="remove">刪除</button>
    <span :class="{ 'text--line-through': item.done }">{{ item.text }}</span>
  </li>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";

const props = defineProps(["item"]);
const emit = defineEmits(["todo:delete"]);

function remove() {
  emit("todo:delete", props.item);
}

function complete() {
  const { item } = props;
  item.done = !item.done;
}
</script>

<style scoped>
.text--line-through {
  text-decoration: line-through;
}
button {
  margin: 0 0.2em;
}
</style>
