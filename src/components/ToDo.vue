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
  newTask.value = ''
}

const updateTask = (id: number, status: 'todo' | 'in-progress' | 'complete') => {
  const task = tasks.value.find((t) => t.id === id)
  if (task) {
    task.status = status
  }
  localStorage.tasks = JSON.stringify(tasks.value)
}

const removeTask = (id: number) => {
  tasks.value.splice(
    tasks.value.findIndex((task) => task.id === id),
    1,
  )
  localStorage.tasks = JSON.stringify(tasks.value)
}

onMounted(() => {
  const storedTasks = localStorage.tasks ? JSON.parse(localStorage.tasks) : []
  tasks.value = storedTasks
})
</script>

<template>
  <div class="task-app-container">
    <div class="task-app">
      <h2>Задачи</h2>
      <div class="input-container">
        <input v-model="newTask" placeholder="Добавить задачу" @keyup.enter="addTask('todo')" />
        <button @click="addTask('todo')" class="primary-btn">Добавить</button>
      </div>
      <div class="task-sections">
        <div class="task-section">
          <h3>Задачи</h3>
          <ul class="task-list">
            <li
              v-for="(task, index) in tasks.filter((t) => t.status === 'todo')"
              :key="task.id"
              class="task-item todo"
            >
              <span class="task-text">{{ task.title }}</span>
              <div class="task-actions">
                <button @click="updateTask(task.id, 'in-progress')" class="success-btn">
                  В процессе
                </button>
                <button @click="updateTask(task.id, 'complete')" class="success-btn">
                  Выполнено
                </button>
                <button @click="removeTask(task.id)" class="danger-btn">Удалить</button>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>

    <div class="task-app">
      <h2>В процессе</h2>
      <div class="input-container">
        <input
          v-model="newTask"
          placeholder="Добавить задачу"
          @keyup.enter="addTask('in-progress')"
        />
        <button @click="addTask('in-progress')" class="primary-btn">Добавить</button>
      </div>
      <div class="task-sections">
        <div class="task-section">
          <h3>В процессе</h3>
          <ul class="task-list">
            <li
              v-for="(task, index) in tasks.filter((t) => t.status === 'in-progress')"
              :key="task.id"
              class="task-item in-progress"
            >
              <span class="task-text">{{ task.title }}</span>
              <div class="task-actions">
                <button @click="updateTask(task.id, 'todo')" class="secondary-btn">Задача</button>
                <button @click="updateTask(task.id, 'complete')" class="success-btn">
                  Выполнено
                </button>
                <button @click="removeTask(task.id)" class="danger-btn">Удалить</button>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>

    <div class="task-app">
      <h2>Выполненные</h2>
      <div class="input-container">
        <input v-model="newTask" placeholder="Добавить задачу" @keyup.enter="addTask('complete')" />
        <button @click="addTask('complete')" class="primary-btn">Добавить</button>
      </div>
      <div class="task-sections">
        <div class="task-section">
          <h3>Выполненные</h3>
          <ul class="task-list">
            <li
              v-for="(task, index) in tasks.filter((t) => t.status === 'complete')"
              :key="task.id"
              class="task-item complete"
            >
              <span class="task-text">{{ task.title }}</span>
              <div class="task-actions">
                <button @click="updateTask(task.id, 'todo')" class="secondary-btn">Задача</button>
                <button @click="updateTask(task.id, 'in-progress')" class="success-btn">
                  В процессе
                </button>
                <button @click="removeTask(task.id)" class="danger-btn">Удалить</button>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.task-app-container {
  display: flex;
  justify-content: center;
  gap: 20px;
}

.task-app {
  max-width: 400px;
  padding: 30px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #f9f9f9;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.task-app h2 {
  text-align: center;
}

.input-container {
  display: flex;
  flex-direction: column;
}

input {
  padding: 12px;
  border: none;
  border-radius: 4px;
  box-shadow: inset 0 -1px #ccc;
}

input::placeholder {
  color: #aaa;
}

input:focus {
  outline: none;
}

.primary-btn,
.secondary-btn,
.success-btn,
.danger-btn {
  padding: 12px;
  border-radius: 4px;
}

.primary-btn {
  background-color: #007bff;
  color: white;
  border: none;
}
.primary-btn:hover {
  background-color: #0056b3;
}

.secondary-btn {
  background-color: #6c757d;
  color: white;
  border: none;
}
.secondary-btn:hover {
  background-color: #5a6268;
}

.success-btn {
  background-color: #28a745;
  color: white;
  border: none;
}
.success-btn:hover {
  background-color: #218838;
}

.danger-btn {
  background-color: #dc3545;
  color: white;
  border: none;
}
.danger-btn:hover {
  background-color: #c82333;
}

.task-list {
  list-style-type: none;
  padding-left: 0;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px;
  margin-bottom: 10px;
  border-radius: 4px;
  transition: background-color 0.3s ease-in-out;
}

.task-item.todo {
  background-color: #e7f1ff;
}
.task-item.in-progress {
  background-color: #e6f7ff;
}
.task-item.complete {
  background-color: #d4edda;
}
.task-item:hover {
  background-color: #f0f0f0;
}
.task-text {
  color: #333;
}
</style>
