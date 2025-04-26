<script setup>
import { ref, computed } from 'vue';

const newTask = ref('');
const tasks = ref([]);
const selectedFilter = ref('all');
let nextId = 1;

const filteredTasks = computed(() => {
  switch (selectedFilter.value) {
    case 'active': return tasks.value.filter(t => !t.completed);
    case 'completed': return tasks.value.filter(t => t.completed);
    default: return tasks.value;
  }
});

const addTask = () => {
  if (newTask.value.trim()) {
    tasks.value.push({ id: nextId++, text: newTask.value.trim(), completed: false });
    newTask.value = '';
  }
};

const deleteTask = id => tasks.value = tasks.value.filter(t => t.id !== id);
const toggleComplete = id => {
  const t = tasks.value.find(t => t.id === id);
  if (t) t.completed = !t.completed;
};
</script>

<template>
  <div class="wrapper">
    <div class="container">
      <h1>Todo List <i class="fas fa-clipboard-list"></i></h1>

      <!-- Input Card -->
      <div class="card input-card">
        <input
          v-model="newTask"
          @keyup.enter="addTask"
          placeholder="Tambahkan tugas baru..."
        />
        <button @click="addTask">
          <i class="fas fa-plus"></i> Tambah
        </button>
      </div>

      <!-- Filter Card -->
      <div class="card filter-card">
        <select v-model="selectedFilter">
          <option value="all">Semua</option>
          <option value="active">Belum Selesai</option>
          <option value="completed">Selesai</option>
        </select>
        <i class="fas fa-filter"></i>
      </div>

      <!-- List Card -->
      <div class="card list-card">
        <div
          v-for="t in filteredTasks"
          :key="t.id"
          class="task-item"
        >
          <i
            :class="t.completed ? 'fas fa-check-circle' : 'far fa-circle'"
            @click="toggleComplete(t.id)"
          ></i>
          <span :class="{ completed: t.completed }">{{ t.text }}</span>
          <i class="fas fa-trash-alt delete-icon" @click="deleteTask(t.id)"></i>
        </div>
        <!-- Jika tidak ada tugas -->
        <p v-if="filteredTasks.length === 0" class="empty">Belum ada tugas.</p>
      </div>

      <!-- Footnote -->
      <footer class="footnote">
        Made by PacenZein
      </footer>
    </div>
  </div>
</template>

<style>
:root {
  --base: #2e2e2e;
  --light: #383838;
  --dark: #232323;
  --accent: #81c784;
  --accent-dark: #66bb6a;
  --text: #e0e0e0;
}

html, body {
  height: 100%;
  margin: 0;
  background: var(--base);
  font-family: 'Segoe UI', sans-serif;
  color: var(--text);
}

/* Wrapper untuk memusatkan */
.wrapper {
  min-height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
}

/* Container utama */
.container {
  background: var(--base);
  border-radius: 30px;
  box-shadow: 8px 8px 16px var(--dark), -8px -8px 16px var(--light);
  width: 100%;
  max-width: 600px;
  padding: 2rem;
  display: flex;
  flex-direction: column;
}

/* Judul */
h1 {
  text-align: center;
  color: var(--accent);
  margin-bottom: 1.5rem;
  font-size: 2rem;
}

/* Base card */
.card {
  background: var(--base);
  border-radius: 20px;
  box-shadow: 8px 8px 16px var(--dark), -8px -8px 16px var(--light);
  padding: 0.5rem 1rem;
  margin-bottom: 1.5rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

/* Input card */
.input-card input {
  flex: 1;
  padding: 0.6rem 1rem;
  border: none;
  border-radius: 10px;
  background: var(--base);
  box-shadow: inset 4px 4px 8px var(--dark), inset -4px -4px 8px var(--light);
  font-size: 1rem;
  color: var(--text);
}

.input-card button {
  background: var(--accent);
  border: none;
  border-radius: 15px;
  padding: 0.6rem;
  box-shadow: 4px 4px 8px var(--dark), -4px -4px 8px var(--light);
  cursor: pointer;
  color: var(--light);
  font-weight: 600;
}

/* Filter card */
.filter-card select {
  color : var(--text);
  flex: 1;
  padding: 0.4rem 0.75rem;
  border: none;
  border-radius: 15px;
  background: var(--base);
  box-shadow: inset 4px 4px 8px var(--dark), inset -4px -4px 8px var(--light);
  color: var(--text);
  font-size: 1rem;
}

.filter-card i {
  font-size: 1.2rem;
  color: var(--text);
  padding-right: 0.5rem;
}

/* List card */
.list-card {
  flex-direction: column;
  padding: 1rem;
}

.task-item {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 0.6rem;
  border-radius: 15px;
  background: var(--base);
  box-shadow: 4px 4px 8px var(--dark), -4px -4px 8px var(--light);
  margin-bottom: 1rem;
}

.task-item i {
  cursor: pointer;
  font-size: 1.2rem;
  color: var(--accent-dark);
}

.task-item span {
  flex: 1;
  color: var(--text);
}

.task-item span.completed {
  text-decoration: line-through;
  color: var(--accent-dark);
}

.delete-icon {
  color: #bbb;
}

/* Pesan kosong */
.empty {
  text-align: center;
  color: var(--text);
  opacity: 0.7;
  font-style: italic;
}

/* Footnote */
.footnote {
  text-align: center;
  font-size: 0.8rem;
  color: var(--text);
  margin-top: auto;
  opacity: 0.6;
}

/* Responsive adjustments */
@media (max-width: 480px) {
  .container {
    padding: 1rem;
  }
  h1 {
    font-size: 1.5rem;
    margin-bottom: 1rem;
  }
  .card {
    flex-direction: column;
    padding: 0.5rem;
    gap: 0.25rem;
  }
  .input-card input, .filter-card select {
    width: 100%;
  }
  .input-card button, .filter-card i {
    align-self: flex-end;
  }
  .task-item {
    padding: 0.5rem;
    gap: 0.5rem;
  }
}
</style>
