<template>
  <div class="filters">
    <SearchBar v-model="localFilters.name" />
    <StatusFilter v-model="localFilters.status" />
    <SpeciesFilter v-model="localFilters.species" />
  </div> 
</template>

<script setup>
import { computed } from 'vue'
import SearchBar from './SearchBar.vue'
import StatusFilter from './StatusFilter.vue'
import SpeciesFilter from './SpeciesFilter.vue'

const props = defineProps({
  filters: Object
})

const emit = defineEmits(['update:filters'])

const localFilters = computed({
  get: () => props.filters,
  set: (value) => emit('update:filters', value)
})
</script>

<style scoped>
.filters {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}
</style>