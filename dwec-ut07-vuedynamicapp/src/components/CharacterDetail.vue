<template>
  <div class="overlay" @click.self="close">

    <div class="modal">
      <button class="close-btn" @click="close">✖</button>

      <div class="character-info">

        <img :src="character.image" />
        
        <h2>{{ character.name }}</h2>
        <p>Especie: {{ character.species }}</p>
        <p>Estado: {{ character.status }}</p>
        <p>Género: {{ character.gender }}</p>
      </div>

      <div class="location">
        <h3>Localización</h3>

        <p v-if="loadingLocation">Cargando localización...</p>

        <p v-else-if="locationError">
          {{ locationError }}
        </p>

        <div v-else-if="location">
          <p><strong>Nombre:</strong> {{ location.name }}</p>
          <p><strong>Tipo:</strong> {{ location.type }}</p>
          <p><strong>Dimensión:</strong> {{ location.dimension }}</p>
          <p><strong>Residentes:</strong> {{ location.residents.length }}</p>
        </div>
      </div>

    </div>

    

  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  character: Object
})

const location = ref(null)
const loadingLocation = ref(false)
const locationError = ref(null)

const fetchLocation = async () => {
  if (!props.character?.location?.url) return

  loadingLocation.value = true
  locationError.value = null

  try {
    const res = await fetch(props.character.location.url)

    if (!res.ok) {
      throw new Error('Error al cargar localización')
    }

    const data = await res.json()
    location.value = data

  } catch (e) {
    console.error(e)
    locationError.value = 'No se pudo cargar la localización'
  } finally {
    loadingLocation.value = false
  }
}

const emit = defineEmits(['close'])

const close = () => emit('close')

const handleKey = (e) => {
  if (e.key === 'Escape') {
    close()
  }
}

onMounted(() => {
  console.log("CharacterDetail: ", props.character)
  window.addEventListener('keydown', handleKey)
  fetchLocation()
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
  display: flex;
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