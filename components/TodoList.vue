<template lang="pug">
  .container.mx-auto.max-w-xl.p-6.bg-emerald-100.shadow-lg.rounded-lg.mt-10
    .task.space-y-6
      //- title
      .title.text-center
        h1.text-3xl.font-bold.text-blue-600 To Do List

      //- form
      .form.flex.items-center.space-x-2
        input(
          type="text",
          placeholder="New Task",
          v-model="newTask",
          @keyup.enter="addTask",
          class="flex-1 px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-400"
        )
        button(
          @click="addTask",
          class="bg-blue-500 text-white p-2 rounded hover:bg-blue-600 transition"
        )
          i.fas.fa-plus

      //- task lists
      .taskItems
        ul.space-y-2
          task-item(
            v-for="(task, index) in tasks",
            :key="task.id",
            :task="task",
            @remove="removeTask(index)",
            @complete="completeTask(task)"
          )

      //- buttons
      .clearBtns.flex.justify-between
        button(
          @click="clearCompleted",
          class="bg-yellow-400 text-white px-4 py-2 rounded hover:bg-yellow-500 transition"
        ) Clear completed
        button(
          @click="clearAll",
          class="bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600 transition"
        ) Clear all

      //- pending task
      .pendingTasks.text-right.text-gray-600
        span.text-sm.font-medium Pending Tasks: {{ incomplete }}
</template>


<script setup>
import { ref, computed } from 'vue'
import TaskItem from './Task-item.vue'

// Props
const props = defineProps({
  tasks: {
    type: Array,
    required: true
  }
})

// Emits
const emit = defineEmits(['update:tasks'])

// Local state
const newTask = ref('')

// Computed
const incomplete = computed(() =>
  props.tasks.filter(task => !task.completed).length
)

// Methods
function addTask() {
  if (newTask.value) {
    const newTasks = [
      ...props.tasks,
      {
        id: Date.now(), // Add unique ID
        title: newTask.value,
        completed: false,
      }
    ]
    emit('update:tasks', newTasks)
    newTask.value = ''
  }
}

function completeTask(task) {
  const newTasks = props.tasks.map(t =>
    t === task ? { ...t, completed: !t.completed } : t
  )
  emit('update:tasks', newTasks)
}

function removeTask(index) {
  const newTasks = props.tasks.filter((_, i) => i !== index)
  emit('update:tasks', newTasks)
}

function clearCompleted() {
  const newTasks = props.tasks.filter(task => !task.completed)
  emit('update:tasks', newTasks)
}

function clearAll() {
  emit('update:tasks', [])
}
</script>


  