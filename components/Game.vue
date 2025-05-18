<template>
  <div>
    <GameBoard
      :scores="scores"
      :userChoice="userChoice"
      :cpuChoice="cpuChoice"
      :outcome="outcome"
      :roundLimit="roundLimit"
      @play="handlePlay"
      @reset="resetGame"
      @update:roundLimit="roundLimit = $event"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import GameBoard from './GameBoard.vue'

// Inicializar las variables reactivas con valores por defecto
const userChoice = ref(null)
const cpuChoice = ref(null)
const outcome = ref('')
const roundLimit = ref(3)

// Inicializar scores con un objeto completo desde el principio
const scores = ref({
  user: 0,
  cpu: 0,
  tie: 0
})

const opciones = ['piedra', 'papel', 'tijeras']

function obtenerGanador(usuario, computadora) {
  if (usuario === computadora) return 'Empate'
  if (
    (usuario === 'piedra' && computadora === 'tijeras') ||
    (usuario === 'tijeras' && computadora === 'papel') ||
    (usuario === 'papel' && computadora === 'piedra')
  ) {
    return 'Ganaste'
  }
  return 'Perdiste'
}

function handlePlay(eleccionUsuario) {
  const eleccionComputadora = opciones[Math.floor(Math.random() * opciones.length)]
  
  userChoice.value = eleccionUsuario
  cpuChoice.value = eleccionComputadora
  
  const resultado = obtenerGanador(eleccionUsuario, eleccionComputadora)
  outcome.value = resultado
  
  // Actualizar scores de manera más segura
  if (resultado === 'Ganaste') {
    scores.value.user++
  } else if (resultado === 'Perdiste') {
    scores.value.cpu++
  } else {
    scores.value.tie++
  }
}

function resetGame() {
  // Resetear manteniendo la estructura del objeto
  scores.value.user = 0
  scores.value.cpu = 0
  scores.value.tie = 0
  
  userChoice.value = null
  cpuChoice.value = null
  outcome.value = ''
}
</script>
