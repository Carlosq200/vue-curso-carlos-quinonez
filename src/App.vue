<script setup>
import { ref, computed, watch, onMounted } from 'vue'
import StatsBar from './components/StatsBar.vue'
import TodoItem from './components/TodoItem.vue'

const nuevaTarea = ref('')
const tareas = ref([]) // { id:number, texto:string, done:boolean }
const filtro = ref('todas') // 'todas' | 'pendientes' | 'completadas'

// Cargar desde localStorage al iniciar
onMounted(() => {
  const guardadas = localStorage.getItem('tareas')
  if (guardadas) tareas.value = JSON.parse(guardadas)
})

// Guardar automáticamente ante cualquier cambio profundo
watch(tareas, (nuevo) => {
  localStorage.setItem('tareas', JSON.stringify(nuevo))
}, { deep: true })

// Computed
const total = computed(() => tareas.value.length)
const completadas = computed(() => tareas.value.filter(t => t.done).length)
const pendientes = computed(() => total.value - completadas.value)

const tareasFiltradas = computed(() => {
  if (filtro.value === 'pendientes') return tareas.value.filter(t => !t.done)
  if (filtro.value === 'completadas') return tareas.value.filter(t => t.done)
  return tareas.value
})

// Métodos
function agregarTarea() {
  const texto = nuevaTarea.value.trim()
  if (!texto) return
  tareas.value.push({ id: Date.now(), texto, done: false })
  nuevaTarea.value = ''
}
function toggleTarea(id) {
  const t = tareas.value.find(x => x.id === id)
  if (t) t.done = !t.done
}
function eliminarTarea(id) {
  tareas.value = tareas.value.filter(t => t.id !== id)
}
function limpiarCompletadas() {
  tareas.value = tareas.value.filter(t => !t.done)
}
</script>

<template>
  <main style="max-width:780px;margin:2rem auto;font-family:system-ui;">
    <h1>To-Do Vue · Composition API</h1>

    <form @submit.prevent="agregarTarea" style="display:flex; gap:.5rem;">
      <input v-model="nuevaTarea" placeholder="Nueva tarea..." style="flex:1;padding:.6rem;" />
      <button type="submit">Agregar</button>
    </form>

    <StatsBar :total="total" :completadas="completadas" :pendientes="pendientes" />

    <nav style="margin:.75rem 0; display:flex; gap:.5rem; flex-wrap:wrap;">
      <button :aria-pressed="filtro==='todas'" @click="filtro='todas'">Todas</button>
      <button :aria-pressed="filtro==='pendientes'" @click="filtro='pendientes'">Pendientes</button>
      <button :aria-pressed="filtro==='completadas'" @click="filtro='completadas'">Completadas</button>
    </nav>

    <ul style="list-style:none;padding:0;margin-top:1rem;">
      <TodoItem
        v-for="t in tareasFiltradas"
        :key="t.id"
        :tarea="t"
        @toggle="toggleTarea"
        @remove="eliminarTarea"
      />
    </ul>

    <div style="margin-top:.5rem; display:flex; gap:.5rem;">
      <button @click="limpiarCompletadas">Eliminar completadas</button>
    </div>

    <small style="display:block;margin-top:1rem;opacity:.8;">
      *Se guarda automáticamente en tu navegador (localStorage).
    </small>
  </main>
</template>

<style>
button { padding:.6rem .8rem; cursor:pointer; }
button[aria-pressed="true"] { outline: 2px solid #333; }
</style>
