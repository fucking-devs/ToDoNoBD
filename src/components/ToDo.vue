<script setup lang="ts">
import { onMounted, ref } from 'vue'
interface Task {
  id: number
  title: string
  status: 'todo' | 'in-progress' | 'complete'
}
const tasks = ref<Task[]>([])
const newTask = ref('')
const addTask = (status: 'todo' | 'in-progress' | 'complete') => {
const taskId = tasks.value.length + 1
 const data = { id: taskId, title: newTask.value.trim(), status }
 tasks.value.unshift(data)
localStorage.tasks = JSON.stringify(tasks.value)
 newTask.value = '' }
const updateTask = (id: number, status: 'todo' | 'in-progress' | 'complete') => {
const task = tasks.value.find(t => t.id === id)
if (task) {   task.status = status}
 localStorage.tasks = JSON.stringify(tasks.value)}
const removeTask = (id: number) => {
tasks.value.splice(tasks.value.findIndex(task => task.id === id), 1)
localStorage.tasks = JSON.stringify(tasks.value)}
onMounted(() => {
const storedTasks = localStorage.tasks ? JSON.parse(localStorage.tasks) : []
tasks.value = storedTasks})
</script>
<template>
 <div class="task-app">
 <h2>Задачи</h2>
<div class="input-container">
<input v-model="newTask" placeholder="Добавить задачу" @keyup.enter="addTask('todo')" />
<button @click="addTask('todo')">Добавить</button>
 </div>
<div class="task-sections">
 <div class="task-section">
 <h3>Задачи</h3>
 <ul class="task-list">
 <li v-for="(task, index) in tasks.filter(t => t.status === 'todo')" :key="task.id" class="task-item">
<span class="task-text">{{ task.title }}</span>
<div class="task-actions">
 <button @click="updateTask(task.id, 'in-progress')">В процессе</button>
 <button @click="updateTask(task.id, 'complete')">Выполнено</button>
  <button @click="removeTask(task.id)" class="remove-button">Удалить</button>
  </div>
  </li>
  </ul>
</div>
 <div class="task-section">
 <h3>В процессе</h3>
 <ul class="task-list">
<li v-for="(task, index) in tasks.filter(t => t.status === 'in-progress')" :key="task.id" class="task-item">
  <span class="task-text">{{ task.title }}</span>
  <div class="task-actions">
 <button @click="updateTask(task.id, 'todo')">Задача</button>
 <button @click="updateTask(task.id, 'complete')">Выполнено</button>
 <button @click="removeTask(task.id)" class="remove-button">Удалить</button>
 </div>
   </li> </ul></div>
<div class="task-section">
 <h3>Выполнено</h3>
 <ul class="task-list">
 <li v-for="(task, index) in tasks.filter(t => t.status === 'complete')" :key="task.id" class="task-item">
 <span class="task-text">{{ task.title }}</span><div class="task-actions">
 <button @click="updateTask(task.id, 'todo')">Задача</button>
  <button @click="updateTask(task.id, 'in-progress')">В процессе</button>
 <button @click="removeTask(task.id)" class="remove-button">Удалить</button>
   </div>
  </li>
 </ul>
 </div>
 </div>
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
