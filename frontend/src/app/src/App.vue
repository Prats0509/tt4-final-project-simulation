<template>
  <div class="max-w-xl mx-auto p-4">
    <h1 class="text-2xl font-bold mb-4">📝 Task Manager</h1>

    <form @submit.prevent="addTask" class="mb-4 space-y-2">
      <input v-model="newTask.title" placeholder="Title" class="w-full p-2 border" />
      <input v-model="newTask.description" placeholder="Description" class="w-full p-2 border" />
      <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded">Add Task</button>
    </form>

    <ul>
      <li v-for="task in tasks" :key="task.id" class="mb-2 p-2 border rounded flex justify-between items-center">
        <div>
          <h2 class="font-semibold">{{ task.title }}</h2>
          <p>{{ task.description }}</p>
          <p>Status: <strong>{{ task.completed ? '✅ Completed' : '❌ Not Completed' }}</strong></p>
        </div>
        <div class="space-x-2">
          <button @click="toggleTask(task)" class="bg-yellow-400 px-2 py-1 rounded">Toggle</button>
          <button @click="deleteTask(task.id)" class="bg-red-500 text-white px-2 py-1 rounded">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const API_URL = 'http://localhost:5004/api/tasks'  // Update if using Docker later

const tasks = ref([])
const newTask = ref({ title: '', description: '', completed: false })

const fetchTasks = async () => {
  const res = await fetch(API_URL)
  tasks.value = await res.json()
}

const addTask = async () => {
  await fetch(API_URL, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(newTask.value)
  })
  newTask.value = { title: '', description: '', completed: false }
  fetchTasks()
}

const deleteTask = async (id) => {
  await fetch(`${API_URL}/${id}`, { method: 'DELETE' })
  fetchTasks()
}

const toggleTask = async (task) => {
  const updatedTask = { ...task, completed: !task.completed }
  await fetch(`${API_URL}/${task.id}`, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(updatedTask)
  })
  fetchTasks()
}

onMounted(fetchTasks)
</script>
