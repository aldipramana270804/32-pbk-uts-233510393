<template>
  <div class="app">
    <h1>Daftar Kegiatan</h1>

    <form @submit.prevent="addTodo" class="form">
      <input
        ref="inputRef"
        v-model="newTodo"
        type="text"
        placeholder="Tambah kegiatan..."
      />
      <button type="submit">Tambah</button>
    </form>

    <select v-model="filter" class="filter">
      <option value="all">Semua</option>
      <option value="pending">Belum Selesai</option>
      <option value="completed">Selesai</option>
    </select>

    <ul>
      <li
        v-for="(todo, index) in filteredTodos"
        :key="index"
        :class="{ done: todo.completed }"
      >
        <input type="checkbox" v-model="todo.completed" />
        <span>{{ todo.text }}</span>
        <button @click="removeTodo(index)">❌</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from "vue";

const newTodo = ref("");
const filter = ref("all");
const inputRef = ref(null);

const todos = ref([
  { text: "Sarapan", completed: false },
  { text: "Belajar Vue.js", completed: false },
  { text: "Tidur siang", completed: true },
]);

const addTodo = () => {
  const text = newTodo.value.trim();
  if (!text) return;

  const exists = todos.value.some(
    (todo) => todo.text.toLowerCase() === text.toLowerCase()
  );
  if (exists) return;

  todos.value.push({ text, completed: false });
  newTodo.value = "";
  inputRef.value?.focus();
};

const removeTodo = (index) => {
  todos.value.splice(index, 1);
};

const filteredTodos = computed(() => {
  if (filter.value === "pending") {
    return todos.value.filter((todo) => !todo.completed);
  } else if (filter.value === "completed") {
    return todos.value.filter((todo) => todo.completed);
  }
  return todos.value;
});
</script>

<style scoped>
.app {
  max-width: 480px;
  margin: 40px auto;
  padding: 24px;
  background-color: #ffffff;
  border: 1px solid #e0e0e0;
  border-radius: 12px;
  font-family: "Inter", sans-serif;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
}

h1 {
  text-align: center;
  color: #333;
  font-weight: 600;
  margin-bottom: 24px;
  font-size: 24px;
}

.form {
  display: flex;
  gap: 8px;
  margin-bottom: 16px;
}

input[type="text"] {
  flex: 1;
  padding: 10px 12px;
  font-size: 14px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background: white;
  transition: border-color 0.2s;
}

input[type="text"]:focus {
  outline: none;
  border-color: #999;
}

button {
  padding: 10px 16px;
  background-color: #4a90e2;
  color: white;
  font-size: 14px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.2s;
}

button:hover {
  background-color: #357ab8;
}

.filter {
  width: 100%;
  padding: 10px;
  font-size: 14px;
  border-radius: 8px;
  border: 1px solid #ddd;
  background-color: white;
  margin-bottom: 20px;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  display: flex;
  align-items: center;
  padding: 12px;
  border-bottom: 1px solid #eaeaea;
  transition: background-color 0.2s;
}

li:hover {
  background-color: #f5f5f5;
}

li.done span {
  text-decoration: line-through;
  color: #999;
}

li span {
  flex: 1;
  margin-left: 10px;
  font-size: 14px;
}

li input[type="checkbox"] {
  transform: scale(1.2);
}

li button {
  background: none;
  border: none;
  color: #e57373;
  font-size: 16px;
  cursor: pointer;
  padding: 4px;
}

li button:hover {
  color: #c62828;
}
</style>
