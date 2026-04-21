<template>
  <select v-model="model">
    <option value="">Todas las especies</option>
    <option value="human">Humano</option>
    <option value="alien">Alien</option>
    <option value="robot">Robot</option>
    <option value="mythological creature">Mitológico</option>
    <option value="animal">Animal</option>
  </select>
</template>

<script setup>
const model = defineModel()
</script>