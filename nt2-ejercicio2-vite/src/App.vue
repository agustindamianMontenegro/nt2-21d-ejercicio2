<template>
  <div class="container mt-4">
    <input
      type="text"
      v-model="nombreFiltro"
      class="form-control mb-2"
      placeholder="Filtrar por nombre y apellido"
    />
    <input
      type="text"
      v-model="dniFiltro"
      class="form-control mb-2"
      placeholder="Filtrar por DNI"
    />

    <div
      v-if="mostrarAdvertencia"
      class="alert alert-warning"
    >
      Ingresá al menos 3 caracteres en alguno de los filtros.
    </div>

    <div class="row">
      <div
        class="col-md-4"
        v-for="persona in personasFiltradas"
        :key="persona.dni"
      >
        <div class="card mb-3">
          <div class="card-body">
            <h5 class="card-title">{{ getNombreCompleto(persona) }}</h5>
            <p class="card-text">DNI: {{ persona.dni }}</p>
            <a :href="'mailto:' + persona.correo" class="card-link">{{ persona.correo }}</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const nombreFiltro = ref('')
const dniFiltro = ref('')

const personas = ref([
  { nombre: 'Daniel', apellido: 'Sanchez', correo: 'danielsanchez68@hotmail.com', dni: '20442873' },
  { nombre: 'Juan', apellido: 'Perez', correo: 'j@p.gmail.com', dni: '12345678' },
  { nombre: 'Ana', apellido: 'Suarez', correo: 'a@s.gmail.com', dni: '87654321' },
  { nombre: 'Pedro', apellido: 'Lopez', correo: 'p@l.gmail.com', dni: '33445566' },
  { nombre: 'Agustin', apellido: 'Montenegro', correo: 'agustin.damian.montenegro@gmail.com', dni: '4013131' },
])

const mostrarAdvertencia = computed(() => {
  return (
    (nombreFiltro.value && nombreFiltro.value.length < 3) ||
    (dniFiltro.value && dniFiltro.value.length < 3)
  )
})

const personasFiltradas = computed(() => {
  if (nombreFiltro.value.length < 3 && dniFiltro.value.length < 3) return []

  return personas.value.filter((p) => {
    const nombreCompleto = `${p.nombre} ${p.apellido}`.toLowerCase()
    const nombreCoincide =
      nombreFiltro.value.length >= 3 &&
      nombreCompleto.includes(nombreFiltro.value.toLowerCase())

    const dniCoincide =
      dniFiltro.value.length >= 3 &&
      p.dni.includes(dniFiltro.value)

    if (nombreFiltro.value.length >= 3 && dniFiltro.value.length >= 3) {
      return nombreCoincide && dniCoincide
    } else if (nombreFiltro.value.length >= 3) {
      return nombreCoincide
    } else if (dniFiltro.value.length >= 3) {
      return dniCoincide
    }

    return false
  })
})

function getNombreCompleto(persona) {
  return `${persona.nombre} ${persona.apellido}`
}
</script>

<style>
body {
  background-color: #f8f9fa;
}
</style>
