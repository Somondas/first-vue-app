<script setup>
import { onMounted, ref } from "vue";

const name = ref("John Doe");
const status = ref("active");
const tasks = ref(["task 1", "task 2", "task 3"]);
const newtask = ref("");

const toggleStatus = () => {
  if (status.value === "active") {
    status.value = "pending";
  } else if (status.value === "pending") {
    status.value = "inactive";
  } else {
    status.value = "active";
  }
};
const addTask = () => {
  if (newtask.value !== "") {
    tasks.value.push(newtask.value);
    newtask.value = "";
  }
};
const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};
onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos/");
    const data = await response.json();
    tasks.value = data.map((task) => task.title);
  } catch (error) {
    console.log(error);
  }
});
</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is active</p>
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else="status === 'inactive'">User is inactive</p>
  <ul>
    <li v-for="(task, index) in tasks" key="task">
      <span>
        {{ task }}
      </span>

      <button @click="deleteTask(index)">x</button>
    </li>
  </ul>
  <form @submit.prevent="addTask">
    <label>Add task</label>
    <input type="text" v-model="newtask" />
    <button type="submit">add task</button>
  </form>

  <button @click="toggleStatus">Click to</button>
</template>
