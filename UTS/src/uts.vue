<template>
  <div class="outer-wrapper">
    <div class="container">
      <h1>📝 Daftar Kegiatan</h1>

      <form @submit.prevent="addTodo" class="form">
        <div class="input-wrapper">
          <input
            v-model="newTodo"
            type="text"
            placeholder="Tambah kegiatanmu di sini..."
          />
        </div>
        <div class="button-wrapper">
          <button type="submit">➕ Tambah</button>
        </div>
      </form>

      <div class="dropdown-filter">
        <select v-model="filter">
          <option value="all">📃 Semua</option>
          <option value="pending">🕒 Belum</option>
          <option value="completed">✔️ Selesai</option>
        </select>
      </div>

      <ul class="todo-grid">
        <li
          v-for="(todo, index) in filteredTodos"
          :key="'todo-' + index"
          :class="{ done: todo.completed }"
          class="todo-pill"
        >
          <div class="pill-left">
            <input type="checkbox" v-model="todo.completed" />
          </div>
          <div class="pill-content">
            <span>{{ todo.text }}</span>
          </div>
          <button class="delete" @click="removeTodo(index)">🗑️</button>
        </li>
      </ul>
    </div>

    <footer class="footer">
      <marquee behavior="scroll" direction="left">🌊 @ Aldi Pramana 🌊</marquee>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const newTodo = ref("");
const filter = ref("all");

const todos = ref([
  { text: "Sarapan sehat 🍞", completed: false },
  { text: "Belajar Vue.js 🚀", completed: false },
  { text: "Tidur siang 😴", completed: true },
]);

const addTodo = () => {
  const text = newTodo.value.trim();
  if (text === "") return;
  todos.value.push({ text, completed: false });
  newTodo.value = "";
};

const removeTodo = (index) => {
  todos.value.splice(index, 1);
};

const filteredTodos = computed(() => {
  if (filter.value === "pending") {
    return todos.value.filter((todo) => !todo.completed);
  } else if (filter.value === "completed") {
    return todos.value.filter((todo) => todo.completed);
  } else {
    return todos.value;
  }
});
</script>

<style scoped>
.outer-wrapper {
  background: linear-gradient(135deg, #001f3f 0%, #fdf6e3 100%);
  padding: 40px 20px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.container {
  max-width: 960px;
  margin: 0 auto;
  background: rgba(255, 255, 255, 0.85);
  border-radius: 24px;
  padding: 32px;
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.1);
  font-family: "Segoe UI", sans-serif;
  transition: all 0.3s ease;
}

h1 {
  text-align: center;
  color: #001f3f;
  margin-bottom: 24px;
  font-size: 40px;
  font-weight: bold;
}

.form {
  display: flex;
  gap: 16px;
  margin-bottom: 16px;
}

.input-wrapper {
  flex: 1;
}

input[type="text"] {
  width: 100%;
  padding: 12px 20px;
  font-size: 16px;
  border: 2px solid #001f3f;
  border-radius: 12px;
  background: #ffffff;
  outline: none;
  height: 48px;
  box-sizing: border-box;
}

input[type="text"]:focus {
  border-color: #003366;
  background-color: #fefae0;
}

.button-wrapper {
  flex-shrink: 0;
}

button {
  background: #001f3f;
  color: white;
  font-weight: bold;
  border: none;
  padding: 0 24px;
  height: 48px;
  font-size: 16px;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s ease;
}

button:hover {
  background-color: #003366;
}

.dropdown-filter {
  margin-bottom: 20px;
  display: flex;
  justify-content: flex-start;
}

select {
  padding: 10px 14px;
  font-size: 16px;
  border-radius: 12px;
  border: 2px solid #001f3f;
  background-color: #fdf6e3;
  color: #001f3f;
  outline: none;
}

.todo-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 16px;
  padding: 0;
  margin-top: 16px;
}

.todo-pill {
  display: flex;
  align-items: center;
  padding: 14px 20px;
  background: #ffffff;
  border-radius: 999px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  transition: all 0.3s ease;
  border: 1px solid #eee;
}

.todo-pill:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 18px rgba(0, 0, 0, 0.08);
}

.pill-left {
  margin-right: 14px;
}

.pill-content {
  flex: 1;
  font-size: 16px;
  font-weight: 500;
  color: #001f3f;
}

.todo-pill.done {
  background: #fefae0;
  border: 1px solid #ccc;
}

.todo-pill.done span {
  text-decoration: line-through;
  color: #777;
}

.delete {
  background: transparent;
  border: none;
  cursor: pointer;
  font-size: 18px;
  transition: 0.2s ease;
  color: #ff6b6b;
}

.delete:hover {
  transform: scale(1.2);
}

.footer {
  margin-top: 30px;
  text-align: center;
  font-weight: bold;
  font-size: 14px;
  color: #020e53;
  padding: 12px;
}
</style>
