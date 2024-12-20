<script setup lang="ts">
import { defineComponent, ref } from 'vue'

const newTask = ref('')
const Tasks = ref<string[]>([])
const addTask = () => {
  if (newTask.value.trim()) {
    Tasks.value.push(newTask.value)
    newTask.value = ''
  }
}
const removeTask = (index: number) => {
  Tasks.value.splice(index, 1)
}
</script>

<template>
  <div>
    <h2>To do</h2>
    <input v-model="newTask" @keyup.enter="addTask" placeholder="Добавить задачу" />
    <ul>
      <li v-for="(Todo, index) in Tasks" :key="index">
        {{ Todo }}
        <button @click="removeTask(index)">Удалить</button>
      </li>
    </ul>
  </div>
</template>

<style>

</style>
