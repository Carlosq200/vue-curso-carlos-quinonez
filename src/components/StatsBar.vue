<script setup>
import { computed } from 'vue'

const props = defineProps({
  total: { type: Number, required: true },
  completadas: { type: Number, required: true },
  pendientes: { type: Number, required: true }
})

const resumen = computed(() => `Completadas: ${props.completadas} / Pendientes: ${props.pendientes}`)
</script>

<template>
  <section style="margin-top:1rem;">
    <p><strong>Total:</strong> {{ props.total }} | <strong>{{ resumen }}</strong></p>
  </section>
</template>
