<template>
    <div class="character-list">
        <CharacterCard
            v-for="character in characters"
            :key="character.id"
            :character="character"
            @show-detail="$emit('show-detail', $event)"
            />
    </div>
</template>

<script setup>
import CharacterCard from './CharacterCard.vue'
const props = defineProps({
  characters: Array
})

</script>