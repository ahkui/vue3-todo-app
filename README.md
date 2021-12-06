# Vue 3

### 準備環境
#### 安裝 NVM - NodeJS 版本管理器
```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
```
> 將以下內容添加至 `.zshrc` or `.bashrc`
> `export NVM_DIR="$HOME/.nvm"`
> `[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm`

> 若需移除 NodeJS 只需將該文件夾刪除即可 `rm -r ~/.nvm`

#### 安裝 NodeJS
```shell
nvm install 16
nvm use 16
```

#### 安裝 Vue CLI
```shell
npm i -g @vue/cli
```

> 推薦使用 `VSCode + misterj.vue-volar-extention-pack`

---

### 建立 Vue3 專案 + 運行
```shell
vue create -p '__default_vue_3__' hello-vue3
cd hello-vue3
npm run serve
```

---

### 第一章 - Vue 基礎
#### 1. 宣告變數並顯示在頁面上
將 `src/App.vue` 替換以下內容
```vue
<template>
  <h1>Hello {{ world }}</h1>
</template>

<script setup>
const world = "world";
</script>
```

#### 2. 利用 Input 替換 `world`
將 `src/App.vue` 替換以下內容
```vue
<template>
  <h1>Hello {{ world }}</h1>
  <input type="text" v-model="world" />
</template>

<script setup>
import { ref } from "vue";

const world = ref("world");
</script>
```

---

### 第二章 - 基本 TodoList
#### 1. 宣告陣列並用 `ul li` 將內容顯示
將 `src/App.vue` 替換以下內容
```vue
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
```

#### 2. 利用 `v-for` 替代重複內容
將 `src/App.vue` 替換以下內容
```vue
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
```

#### 3. 利用 `input` 為 todo 添加內容
將 `src/App.vue` 替換以下內容
```vue
<template>
  <div>
    <input type="text" v-model="todoInput" />
    <button @click="addTodo">Add</button>

    <ul>
      <li v-for="item in todos" :key="item.id">{{ item.text }}</li>
    </ul>
  </div>
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
```

---

### 第三章 - 完善 TodoList
#### 1. 將 TodoList 包成 Component
建立`src/components/TodoList.vue` 並將`src/App.vue`內容複製過來
```vue
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

```
將`src/App.vue` 替換以下內容
```vue
<template>
  <div>
    <todo-list></todo-list>
  </div>
</template>

<script setup>
import TodoList from './components/TodoList.vue';
</script>
```

#### 2. 將 TodoItem 包成 Component
建立`src/components/TodoItem.vue` 並填入以下內容
```vue
<template>
  <li>{{ item.text }}</li>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps(['item']);

</script>
```

將`src/components/TodoList.vue` 替換以下內容
```vue
<template>
  <input type="text" v-model="todoInput" />
  <button @click="addTodo">Add</button>

  <ul>
    <todo-item v-for="item in todos" :key="item.id" :item="item"></todo-item>
  </ul>
</template>

<script setup>
import { ref } from "vue";

import TodoItem from "./TodoItem.vue";

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
```

#### 3. TodoItem 可以標記完成或刪除
將`src/components/TodoItem.vue` 替換以下內容
```vue
<template>
  <li>
    <button @click="remove">刪除</button>
    {{ item.text }}
  </li>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";

const props = defineProps(['item']);
const emit = defineEmits([
  'todo:delete',
])

function remove() {
  emit('todo:delete', props.item)
}
</script>
```

將`src/components/TodoList.vue` 替換以下內容
```vue
<template>
  <input type="text" v-model="todoInput" />
  <button @click="addTodo">Add</button>

  <ul>
    <todo-item v-for="item in todos" :key="item.id" :item="item" @todo:delete="deleteTodo"></todo-item>
  </ul>
</template>

<script setup>
import { ref } from "vue";

import TodoItem from "./TodoItem.vue";

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

function deleteTodo(item) {
  todos.value = todos.value.filter(todo => todo.id != item.id)
}
</script>
```

#### 4. 動態計算當前 Todo 數量
將`src/components/TodoList.vue` 替換以下內容
```vue
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
      @todo:complete="completeTodo"
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
  todos.value = todos.value.filter(todo => todo.id != item.id)
}

function completeTodo(item) {
  item.done = !item.done
}
</script>
```

---

### 附錄
- [重新認識 JavaScript](https://ithelp.ithome.com.tw/users/20065504/ironman/1259)
- [NVM - Github](https://github.com/nvm-sh/nvm)
- [Vue 3 官方文件](https://v3.cn.vuejs.org/guide)
