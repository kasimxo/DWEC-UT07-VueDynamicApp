<template>
  <div class="overlay" @click.self="close">

    <div class="modal">
      <button class="close-btn" @click="close">✖</button>

      <img :src="character.image" />

      <h2>{{ character.name }}</h2>
      <p>Especie: {{ character.species }}</p>
      <p>Estado: {{ character.status }}</p>
      <p>Género: {{ character.gender }}</p>
    </div>

  </div>
</template>

<script setup>
import { onMounted, onUnmounted } from 'vue'

const props = defineProps({
  character: Object
})

const emit = defineEmits(['close'])

const close = () => emit('close')

// cerrar con ESC
const handleKey = (e) => {
  if (e.key === 'Escape') {
    close()
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKey)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKey)
})
</script>

<style scoped>
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.6);

  display: flex;
  justify-content: center;
  align-items: center;

  z-index: 1000;
}

.modal {
  background: white;
  padding: 20px;
  border-radius: 10px;
  max-width: 400px;
  text-align: center;
  position: relative;
  animation: fadeIn 0.2s ease;
}

@keyframes fadeIn {
  from { transform: scale(0.9); opacity: 0; }
  to { transform: scale(1); opacity: 1; }
}

.modal img {
  width: 200px;
  border-radius: 10px;
}

.close-btn {
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
}
</style>