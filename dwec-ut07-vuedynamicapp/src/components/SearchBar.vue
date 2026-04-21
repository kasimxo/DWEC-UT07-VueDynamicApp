<template>
  <input
    v-model="model"
    type="text"
    placeholder="Buscar personaje..."
  />
</template>

<script setup>
const model = defineModel()
</script>