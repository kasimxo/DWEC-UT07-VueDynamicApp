<template>
  <div class="overlay" @click.self="close">

    <div class="modal">
      <button class="close-btn" @click="close">✖</button>
      <div class="top-section">

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

      <div class="episodes">
        <h3>Episodios</h3>

        <p v-if="loadingEpisodes">Cargando episodios...</p>

        <p v-else-if="episodesError">
          {{ episodesError }}
        </p>

        <ul v-else>
          <li v-for="ep in episodes" :key="ep.id">
            <strong>{{ ep.name }}</strong>
            ({{ ep.episode }}) - {{ ep.air_date }}
          </li>
        </ul>
      </div>

    </div>

    

  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, watch } from 'vue'

const props = defineProps({
  character: Object
})

const episodes = ref([])
const loadingEpisodes = ref(false)
const episodesError = ref(null)

const getEpisodeIds = () => {
  return props.character.episode
    .map(url => url.split('/').pop())
    .join(',')
}

const fetchEpisodes = async () => {
  if (!props.character?.episode?.length) return

  loadingEpisodes.value = true
  episodesError.value = null

  try {
    const ids = getEpisodeIds()

    const res = await fetch(
      `https://rickandmortyapi.com/api/episode/${ids}`
    )

    if (!res.ok) {
      throw new Error('Error al cargar episodios')
    }

    const data = await res.json()

    episodes.value = Array.isArray(data) ? data : [data]

  } catch (e) {
    console.error(e)
    episodesError.value = 'No se pudieron cargar los episodios'
  } finally {
    loadingEpisodes.value = false
  }
}

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

watch(
  () => props.character,
  () => {
    fetchLocation()
    fetchEpisodes()
  },
  { immediate: true }
)

onMounted(() => {
  console.log("CharacterDetail: ", props.character)
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
  position: relative;
  animation: fadeIn 0.2s ease;

  display: flex;
  flex-direction: column; 
  gap: 20px;
  max-width: 700px;
  width: 90%;
  max-height: 90vh;
  overflow-y: auto;
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

.episodes {
  width: 100%;
}

.top-section {
  display: flex;
  gap: 20px;
  align-items: stretch;
}

.character-info,
.location {
  flex: 1;
  text-align: left;
}

.location {
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}
</style>