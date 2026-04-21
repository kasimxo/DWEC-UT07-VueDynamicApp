<template>
  <select v-model="model">
    <option value="">Todos</option>
    <option value="alive">Vivos</option>
    <option value="dead">Muertos</option>
    <option value="unknown">Desconocido</option>
  </select>
</template>

<script setup>
const model = defineModel()
</script>