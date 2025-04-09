<template lang="pug">
    li.flex.items-center.justify-between.bg-gray-100.p-3.rounded-md.shadow-sm
      button(
        @click.self="$emit('complete')",
        :class="[task.completed ? 'line-through' : '', 'flex items-center gap-2 text-left text-gray-800 hover:text-blue-600 transition']"
      )
        | {{ task.title }}
        
      button(@click="$emit('remove')" class="text-red-500 hover:text-red-700 transition")
        i.far.fa-trash-alt
</template>
  
<script setup>
import { computed } from 'vue'

const props = defineProps({
  task: {
    type: Object,
    required: true,
  },
})
console.log(props.task);

const className = computed(() => {
  const classes = ['toggle']
  if (props.task.completed) {
    classes.push('toggle-completed')
  }
  return classes.join(' ')
})
</script>
