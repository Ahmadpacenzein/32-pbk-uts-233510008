<script setup>
import { ref, computed } from 'vue';

const newTask = ref('');
const tasks = ref([]);
const selectedFilter = ref('all'); // 'all', 'active', 'completed'
let nextId = 1;

const filteredTasks = computed(() => {
  switch(selectedFilter.value) {
    case 'active':
      return tasks.value.filter(task => !task.completed);
    case 'completed':
      return tasks.value.filter(task => task.completed);
    default:
      return tasks.value;
  }
});

const addTask = () => {
  if (newTask.value.trim()) {
    tasks.value.push({
      id: nextId++,
      text: newTask.value.trim(),
      completed: false
    });
    newTask.value = '';
  }
};

const deleteTask = (id) => {
  tasks.value = tasks.value.filter(task => task.id !== id);
};

const toggleComplete = (id) => {
  const task = tasks.value.find(task => task.id === id);
  if (task) task.completed = !task.completed;
};
</script>

<template>
  <div class="container">
    <h1>Todo List <i class="fas fa-clipboard-list"></i></h1>
    
    <div class="input-section">
      <input
        type="text"
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Tambahkan tugas baru..."
      >
      <button @click="addTask">
        <i class="fas fa-plus"></i> Tambah
      </button>
    </div>

    <div class="filter-section">
      <select v-model="selectedFilter" class="filter-select">
        <option value="all">Semua</option>
        <option value="active">Belum Selesai</option>
        <option value="completed">Selesai</option>
      </select>
      <span class="filter-icon">
        <i class="fas fa-filter"></i>
      </span>
    </div>

    <div class="task-list">
      <div 
        v-for="task in filteredTasks"
        :key="task.id"
        class="task-item"
      >
        <span class="check-icon" @click="toggleComplete(task.id)">
          <i :class="task.completed ? 'fas fa-check-circle' : 'far fa-circle'"></i>
        </span>
        
        <span class="task-text" :class="{ completed: task.completed }">
          {{ task.text }}
        </span>
        
        <button class="delete-btn" @click="deleteTask(task.id)">
          <i class="fas fa-trash-alt"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.filter-section {
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 15px 0;
}

.filter-select {
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background: white;
  cursor: pointer;
}

.filter-icon {
  color: #42b883;
  font-size: 1.1rem;
}

.container {
  max-width: 600px;
  margin: 2rem auto;
  padding: 20px;
}

</style>