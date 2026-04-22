<template>
  <h1>TAREA 7: App dinámica con Vue (API Rick & Morty)</h1>
  <Filters v-model:filters="filters" />
  <Pagination v-model:page="page" :total-pages="totalPages" />
  <p v-if="loading">Cargando...</p>

 <p v-else-if="error">
    {{ error }}
  </p> 

  <div v-else>
    <CharacterList 
      :characters="characters" 
      @show-detail="openCharacter"
    />
  </div>

  <CharacterDetail
    v-if="selectedCharacter"
    :character="selectedCharacter"
    @close="closeCharacter"
  />
</template>

<style scoped>
.character-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}
</style>

<script setup>
import { ref, onMounted, watch } from 'vue'
import CharacterList from './components/CharactersList.vue'
import Filters from './components/Filters.vue'
import Pagination from './components/Pagination.vue'
import CharacterDetail from './components/CharacterDetail.vue'

const characters = ref([])
const loading = ref(false)
const page = ref(1)
const totalPages = ref(1)
const error = ref(null)

const selectedCharacter = ref(null)

const openCharacter = (character) => {
  selectedCharacter.value = character
}

const closeCharacter = () => {
  selectedCharacter.value = null
}

const filters = ref({
  name: '',
  status: '',
  species: '',
})

let controller = null

async function fetchCharacters() {
  if (controller) controller.abort()

  controller = new AbortController()

  error.value = null
  let base_url = 'https://rickandmortyapi.com/api/character'

  let queryParams = []

  if (filters.value.name) {
    queryParams.push(`name=${filters.value.name}`)
  }

  if(filters.value.status) {
    queryParams.push(`status=${filters.value.status}`)
  }

  if(filters.value.species) {
    queryParams.push(`species=${filters.value.species}`)
  }

  if (page.value) {
    queryParams.push(`page=${page.value}`)
  }

  if (queryParams.length > 0) {
    base_url += '?' + queryParams.join('&')
  }

  try {
    const res = await fetch(base_url, {
        signal: controller.signal
      })

    if (!res.ok) {

      if (res.status === 404) {
        error.value = "No se encontraron personajes"
        return []
      } else if(res.status === 429) {
        error.value = "Demasiadas solicitudes. Por favor, inténtalo de nuevo más tarde."
        return []
      } else {
        error.value = "Error inesperado"
        return []
      }
    }

    const data = await res.json()
    totalPages.value = data.info.pages
    return data.results

  } catch (err) {
    if (err.name === 'AbortError') {
      console.log('Fetch aborted')
    } else {
      error.value = "Error de red. Por favor, inténtalo de nuevo."
    }
    return []
  }
  
}

const searchCharacter = async (query) => {
  filters.value.name = query
}

const filterByStatus = async (status) => {
  filters.value.status = status
}

const filterBySpecies = async (species) => {
  filters.value.species = species
}

watch(filters, () => {
  page.value = 1
}, { deep: true })

let timeout = null

watch(
  [filters, page],
  () => {
    loading.value = true
    clearTimeout(timeout)

    timeout = setTimeout(async () => {
      characters.value = await fetchCharacters()
      loading.value = false
    }, 400) 
  },
  { deep: true, immediate: true }
)
</script>

