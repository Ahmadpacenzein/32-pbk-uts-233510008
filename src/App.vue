<script setup>
import { ref } from 'vue';

const newTask = ref('');
const tasks = ref([]);
let nextId = 1;

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

    <div class="task-list">
      <div 
        v-for="task in tasks"
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
.container {
  max-width: 600px;
  margin: 2rem auto;
  padding: 20px;
}

h1 {
  color: #2c3e50;
  text-align: center;
}

.input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

input {
  flex: 1;
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

button {
  background: #42b883;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
}

.task-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.task-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px;
  background: rgba(255, 255, 255, 0.807);
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.check-icon {
  color: #42b883;
  cursor: pointer;
}

.task-text {
  flex: 1;
}


.task-text.completed {
  text-decoration: line-through;
  text-decoration-color: #ff4444;
  text-decoration-thickness: 2px;
  color: #888; 
  transition: all 0.3s ease; 
}

.delete-btn {
  background: transparent;
  color: #ff4444;
  padding: 4px;
  margin-left: auto;
}
</style>