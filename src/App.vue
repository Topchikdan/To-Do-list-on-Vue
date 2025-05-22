<script setup>
import { ref } from 'vue'

const tasks = ref([
  { id: 1, text: 'Пример задачи' }
])

const checkedTaskIds = ref(new Set())
const newTaskText = ref('')

function addTask() {
  const text = newTaskText.value.trim()
  if (!text) return
  tasks.value.push({
    id: Date.now(),
    text
  })
  newTaskText.value = ''
}

function toggleTask(id) {
  if (checkedTaskIds.value.has(id)) {
    checkedTaskIds.value.delete(id)
  } else {
    checkedTaskIds.value.add(id)
  }
}

function deleteTask(id) {
  tasks.value = tasks.value.filter(task => task.id !== id)
  checkedTaskIds.value.delete(id)
}
</script>

<template>
  <h4>Сегодняшние задачи</h4>
  <div class="tasks">
    <div
      v-for="task in tasks"
      :key="task.id"
      class="task"
    >
      <div class="round">
        <input
          type="checkbox"
          :id="'task-' + task.id"
          :checked="checkedTaskIds.has(task.id)"
          @change="toggleTask(task.id)"
        />
        <label :for="'task-' + task.id"></label>
      </div>
      <span :class="{ done: checkedTaskIds.has(task.id) }">{{ task.text }}</span>
      <button @click="deleteTask(task.id)" class="delete-button">✕</button>
    </div>
  </div>

  <div class="add-task">
    <input
      v-model="newTaskText"
      type="text"
      placeholder="Введите задачу"
      @keyup.enter="addTask"
    />
    <button @click="addTask">+</button>
  </div>
</template>

<style scoped>

</style>
