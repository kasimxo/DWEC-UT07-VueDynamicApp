<template>
  <h1>TAREA 7: App dinámica con Vue/Nuxt (API Rick & Morty)</h1>
  <Filters v-model:filters="filters" />
  <p v-if="loading">Cargando...</p>

  <p v-else-if="characters.length === 0">
    No se encontraron personajes
  </p>

  <div v-else>
    <CharacterList :characters="characters" />
  </div>
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

const characters = ref([])
const loading = ref(false)

onMounted(async () => {
  characters.value = await fetchCharacters()
})

const filters = ref({
  name: '',
  status: '',
  species: '',
})

async function fetchCharacters() {
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

  if (queryParams.length > 0) {
    base_url += '?' + queryParams.join('&')
  }

  const res = await fetch(base_url)
  if (!res.ok) {

    if (res.status === 404) {
    console.log('No hay resultados')
    return []
    }
  }

  const data = await res.json()

  return data.results
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

watch(filters, async () => {
  loading.value = true
  characters.value = await fetchCharacters()
  loading.value = false
}, { deep: true })

</script>

