<script setup>
import { onMounted, ref } from 'vue'

const name = ref('John Doe')
const status = ref('active')
const tasks = ref(['task 1', 'task 2'])
const newTasks = ref('')

const toggleStatus = () => {
  if (status.value === 'active') {
    status.value = 'pending'
  } else if (status.value === 'pending') {
    status.value = 'inactive'
  } else {
    status.value = 'active'
  }
}

const addTasks = () => {
  if (newTasks.value.trim() !== '') {
    tasks.value.push(newTasks.value)
    newTasks.value = ''
  } else {
    alert('musnt be empty')
  }
}

onMounted(async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos/')
    const data = await response.json()
    tasks.value = data.map((task) => task.title)
  } catch (error) {
    console.log(error)
  }
})
</script>

<template>
  <h1>Hello World {{ name }}</h1>
  <p v-if="status === 'active'">User is active</p>
  <p v-else-if="status === 'inactive'">User is inactive</p>
  <p v-else>User is pending</p>

  <form @submit.prevent="addTasks">
    <label for="newTasks"> Add Tasks </label>
    <input placeholder="add tasks" v-model="newTasks" type="text" />
    <button type="submit">Submit</button>
  </form>

  <ul>
    <li v-for="task in tasks" :key="task">{{ task }}</li>
  </ul>

  <button @click="toggleStatus">Change Status</button>
</template>
