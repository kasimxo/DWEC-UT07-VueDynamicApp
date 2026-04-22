<template>
    <div class="character-card" @click="showCharacterDetail">
        <img :src="character.image" alt="Imagen de {{ character.name }}" />
        <p>Nombre: {{ character.name }}</p>
        <p>Especie: {{ character.species }}</p>
        <p>Estado: {{ statusIcon[character.status] }} {{ statusText[character.status] }}</p>
    </div>
</template>

<script setup>
const props = defineProps({
  character: Object
})

const emit = defineEmits(['show-detail'])

const showCharacterDetail = () => {
  emit('show-detail', props.character)
}
const statusIcon = {
  Alive: '🟢',
  Dead: '🔴',
  unknown: '⚪'
}
const statusText = {
  Alive: 'Vivo',
  Dead: 'Muerto',
  unknown: 'Desconocido'
}
</script>

<style scoped>
.character-card {
  border: 2px solid #ccc;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  align-items: center;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}
.character-card p {
  margin: 5px 0;
}
.character-card:hover {
  transform: scale(1.05);
  transition: transform 0.3s ease;
  cursor: pointer;
}
</style>