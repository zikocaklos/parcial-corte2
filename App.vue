<template>
  <div class="p-6 max-w-2xl mx-auto">
    <h1 class="text-2xl font-bold mb-6 text-center">🗓️ Gestión de Reservas</h1>

    <!-- Componentes -->
    <FechaHoraReserva v-model:fecha="reserva.fecha" v-model:hora="reserva.hora" />
    <DatosCliente v-model:nombre="reserva.nombre" v-model:telefono="reserva.telefono" />
    <SeleccionMesa v-model:mesaSeleccionada="reserva.mesa" />

    <!-- Resumen -->
    <div class="mt-6 p-4 border rounded bg-gray-50">
      <h2 class="text-lg font-semibold mb-2">📝 Resumen de la Reserva</h2>
      <p><strong>Nombre:</strong> {{ reserva.nombre }}</p>
      <p><strong>Teléfono:</strong> {{ reserva.telefono }}</p>
      <p><strong>Fecha:</strong> {{ reserva.fecha }}</p>
      <p><strong>Hora:</strong> {{ reserva.hora }}</p>
      <p><strong>Mesa:</strong> {{ reserva.mesa }}</p>
    </div>

    <!-- Botón -->
    <button
      class="mt-4 px-4 py-2 bg-green-600 text-white rounded hover:bg-green-700"
      @click="confirmarReserva"
    >
      Confirmar Reserva
    </button>
  </div>
</template>

<script setup>
import { reactive } from 'vue'
import FechaHoraReserva from '@/components/FechaHoraReserva.vue'
import DatosCliente from '@/components/DatosCliente.vue'
import SeleccionMesa from '@/components/SeleccionMesa.vue'

const reserva = reactive({
  fecha: '',
  hora: '',
  nombre: '',
  telefono: '',
  mesa: '',
})

import { crearReserva } from "./service/reservasService.js"


async function confirmarReserva() {
  if (reserva.nombre && reserva.telefono && reserva.fecha && reserva.hora && reserva.mesa) {
    try {
      await crearReserva(reserva)
      alert(`✅ ¡Reserva registrada para ${reserva.nombre} el ${reserva.fecha} a las ${reserva.hora} en la mesa ${reserva.mesa}!`)

      // Limpiar los campos
      reserva.fecha = ''
      reserva.hora = ''
      reserva.nombre = ''
      reserva.telefono = ''
      reserva.mesa = ''
    } catch (error) {
      console.error(error)
      alert('❌ Ocurrió un error al guardar la reserva.')
    }
  } else {
    alert('⚠️ Por favor, completa todos los campos antes de confirmar la reserva.')
  }
}

</script>

<style scoped>
input, select {
  border: 1px solid #ccc;
  padding: 6px;
  margin-top: 4px;
  margin-bottom: 12px;
  border-radius: 4px;
}
</style>
