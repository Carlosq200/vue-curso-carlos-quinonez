<script setup>
const props = defineProps({
  tarea: { type: Object, required: true }
})
const emit = defineEmits(['toggle', 'remove'])
</script>

<template>
  <li :style="{ display:'flex', alignItems:'center', gap:'.5rem', margin:'.25rem 0' }">
    <input type="checkbox" v-model="props.tarea.done" @change="emit('toggle', props.tarea.id)" />
    <span :style="{ textDecoration: props.tarea.done ? 'line-through' : 'none' }">{{ props.tarea.texto }}</span>
    <button @click="emit('remove', props.tarea.id)" aria-label="Eliminar">🗑️</button>
  </li>
</template>
