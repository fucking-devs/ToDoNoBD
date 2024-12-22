<script setup lang="ts">
import { onMounted, ref } from 'vue'

interface Task {
  id: number
  name: string
}

const tasks = ref<Task[]>([])
const newTask = ref('')

const addTask = () => {
  const taskId = tasks.value.length + 1
  const data = { id: taskId, name: newTask.value.trim() }
  tasks.value.unshift(data)
  localStorage.tasks = JSON.stringify(tasks.value)
  newTask.value = ''
}

const updateTask = (id: number) => {
  localStorage.tasks = JSON.stringify(tasks.value)
}

const removeTask = (id: number) => {
  tasks.value.splice(tasks.value.findIndex(task => task.id === id), 1)
  localStorage.tasks = JSON.stringify(tasks.value)
}
</script>
<template>
  <div class="task-app">
    <h2>Задачи</h2>

    <div class="input-container">
      <input v-model="newTask" placeholder="Добавить задачу" @keyup.enter="addTask" />
      <button @click="addTask">Добавить</button>
    </div>

    <ul class="task-list">
      <li v-for="(task, index) in tasks" :key="task.id" class="task-item">
        <input v-model="task.name" class="task-input" @change="updateTask(task.id)"/>
        <button @click="removeTask(task.id)" class="remove-button">Удалить</button>
      </li>
    </ul>
  </div>
</template>



<style>
.task-app {
  max-width: 600px;
  margin: auto;
  padding: 30px;
  border: 1px solid #000;
  border-radius: 8px;
  background-color: #fff;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h2 {
  text-align: center;
  color: #000;
  margin-bottom: 20px;
}

.input-container {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

@media (min-width: 600px) {
  .input-container {
    flex-direction: row;
    justify-content: space-between;
  }
}

input {
  padding: 12px;
  border: 1px solid #000;
  border-radius: 4px;
  width: 100%;
  transition: border-color 0.3s;
}

@media (min-width: 600px) {
  input {
    width: 70%;
  }
}

input:focus {
  border-color: #555;
  outline: none;
}

button {
  padding: 12px 15px;
  border: 1px solid #000;
  border-radius: 4px;
  background-color: #000;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #333;
}

.task-list {
  list-style-type: none;
  padding: 0;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-bottom: 10px;
  background-color: #f9f9f9;
  transition: background-color 0.3s;
}

.task-item:hover {
  background-color: #e0e0e0;
}

.task-input {
  border: none;
  flex: 1;
  padding: 5px;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.task-input:focus {
  background-color: #f1f1f1;
  outline: none;
}

.remove-button {
  background-color: #e53935;
  border: 1px solid #e53935;
}

.remove-button:hover {
  background-color: #c62828;
}
</style>
