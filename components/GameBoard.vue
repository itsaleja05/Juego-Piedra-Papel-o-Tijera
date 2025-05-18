<template>
  <div class="game-container">
    <div class="game-board">
      <h2>🕹️ Piedra, Papel o Tijeras</h2>
      <div class="scoreboard">
        <p>👤 Usuario: {{ props.scores?.user || 0 }}</p>
        <p>🖥️ Computadora: {{ props.scores?.cpu || 0 }}</p>
        <p>🤝 Empates: {{ props.scores?.tie || 0 }}</p>
      </div>
      <div class="choices">
        <button @click="$emit('play', 'piedra')">✊</button>
        <button @click="$emit('play', 'papel')">✋</button>
        <button @click="$emit('play', 'tijeras')">✌️</button>
      </div>
      <div class="result">
        <p>👤 Tú elegiste: {{ userChoiceEmoji }}</p>
        <p>🖥️ La computadora eligió: {{ cpuChoiceEmoji }}</p>
        <p class="outcome">Resultado: <strong>{{ props.outcome }}</strong></p>
      </div>
      <div class="controls">
        <label for="rounds">Mejor de:</label>
        <select
          id="rounds"
          :value="props.roundLimit"
          @change="$emit('update:roundLimit', Number($event.target.value))"
        >
          <option :value="3">3</option>
          <option :value="5">5</option>
          <option :value="7">7</option>
        </select>
        <button @click="$emit('reset')">🔄 Reiniciar Marcador</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

// Definir las props correctamente
const props = defineProps({
  scores: {
    type: Object,
    default: () => ({ user: 0, cpu: 0, tie: 0 })
  },
  userChoice: {
    type: String,
    default: null
  },
  cpuChoice: {
    type: String,
    default: null
  },
  outcome: {
    type: String,
    default: ''
  },
  roundLimit: {
    type: Number,
    default: 3
  }
})

defineEmits(['play', 'reset', 'update:roundLimit'])

const emojis = {
  piedra: '✊',
  papel: '✋',
  tijeras: '✌️',
  null: '❔'
}

const userChoiceEmoji = computed(() => emojis[props.userChoice] || emojis.null)
const cpuChoiceEmoji = computed(() => emojis[props.cpuChoice] || emojis.null)
</script>

<style scoped>
.game-container {
  min-height: 100vh;
  padding: 2rem;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  display: flex;
  align-items: center;
  justify-content: center;
}

.game-board {
  max-width: 500px;
  width: 100%;
  padding: 2rem;
  border: none;
  border-radius: 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1), 0 5px 15px rgba(0, 0, 0, 0.07);
  text-align: center;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: white;
  position: relative;
  overflow: hidden;
}

.game-board::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(45deg, rgba(255,255,255,0.1) 0%, transparent 100%);
  pointer-events: none;
}

.game-board h2 {
  margin-bottom: 1.5rem;
  font-size: 2rem;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
  color: #000000;
  font-weight: bold;
}

.scoreboard {
  background: rgba(255,255,255,0.15);
  border-radius: 15px;
  padding: 1rem;
  margin-bottom: 1.5rem;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255,255,255,0.2);
}

.scoreboard p {
  margin: 0.5rem 0;
  font-size: 1.1rem;
  font-weight: 600;
}

.choices {
  margin: 1.5rem 0;
}

.choices button {
  font-size: 3rem;
  margin: 0.5rem;
  padding: 1rem;
  border-radius: 50%;
  border: none;
  cursor: pointer;
  background: linear-gradient(145deg, #ffffff, #e6e6e6);
  box-shadow: 0 8px 16px rgba(0,0,0,0.1);
  transition: all 0.3s ease;
  width: 80px;
  height: 80px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.choices button:hover {
  transform: translateY(-5px) scale(1.1);
  box-shadow: 0 12px 24px rgba(0,0,0,0.2);
  background: linear-gradient(145deg, #f0f0f0, #d1d1d1);
}

.choices button:active {
  transform: translateY(0) scale(1.05);
}

.result {
  background: rgba(255,255,255,0.1);
  border-radius: 15px;
  padding: 1rem;
  margin: 1.5rem 0;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255,255,255,0.2);
}

.result p {
  margin: 0.5rem 0;
  font-size: 1rem;
}

.outcome {
  font-size: 1.4rem;
  font-weight: bold;
  margin-top: 1rem;
  text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
  color: #ffd700;
}

.controls {
  background: rgba(0,0,0,0.1);
  border-radius: 15px;
  padding: 1rem;
  margin-top: 1.5rem;
}

.controls label {
  font-weight: 600;
  margin-right: 0.5rem;
}

.controls select {
  padding: 0.5rem;
  border-radius: 8px;
  border: none;
  margin-right: 1rem;
  background: white;
  color: #333;
  font-size: 1rem;
}

.controls button {
  padding: 0.75rem 1.5rem;
  border-radius: 25px;
  border: none;
  cursor: pointer;
  background: linear-gradient(45deg, #ff6b6b, #ee5a24);
  color: white;
  font-weight: 600;
  font-size: 1rem;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}

.controls button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(0,0,0,0.2);
  background: linear-gradient(45deg, #ee5a24, #ff6b6b);
}

.controls button:active {
  transform: translateY(0);
}

/* Animación para cuando aparece el resultado */
.outcome {
  animation: resultPulse 0.5s ease-in-out;
}

@keyframes resultPulse {
  0% { transform: scale(0.8); opacity: 0; }
  50% { transform: scale(1.1); }
  100% { transform: scale(1); opacity: 1; }
}
</style>