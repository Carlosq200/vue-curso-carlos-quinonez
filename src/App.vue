<script>
export default {
  name: 'App',
  data() {
    return {
      nombre: ''
    }
  },
  computed: {
    mensajeBienvenida() {
      // Evidencia en consola: se evalúa solo si cambia 'nombre'
      console.count('computed: mensajeBienvenida')
      return this.nombre
        ? `¡Hola, ${this.nombre}! Bienvenido/a 👋`
        : 'Escribe tu nombre para saludarte'
    }
  },
  methods: {
    mensajeBienvenidaMetodo() {
      // Evidencia en consola: se ejecuta en cada render
      console.count('method: mensajeBienvenidaMetodo')
      return this.nombre
        ? `¡Hola, ${this.nombre}! (método)`
        : 'Escribe tu nombre (método)'
    }
  }
}
</script>

<template>
  <main style="max-width:560px;margin:2rem auto;font-family:system-ui;">
    <h1>Semana 1 · Computed vs Method</h1>

    <label>
      Tu nombre:
      <input v-model="nombre" placeholder="Escribe aquí" />
    </label>

    <section style="margin-top:1rem;">
      <h3>Usando computed:</h3>
      <p>{{ mensajeBienvenida }}</p>

      <h3>Usando method:</h3>
      <p>{{ mensajeBienvenidaMetodo() }}</p>
    </section>

    <small>
      Abre la consola (F12 → Console). El <strong>computed</strong> solo recalcula si cambia
      <code>nombre</code>; el <strong>method</strong> se ejecuta en cada render.
    </small>
  </main>
</template>

<style>
input { padding:.5rem; margin-left:.5rem; }
</style>
