<template>
  <div class="pagination">
    <button @click="prevPage" :disabled="page <= 1">
      Página anterior
    </button>

    <p>{{ page }}</p>

    <button @click="nextPage">
      Página siguiente
    </button>
  </div>
</template>

<script setup>
const props = defineProps({
  page: Number
})

const emit = defineEmits(['update:page'])

const prevPage = () => {
  if (props.page > 1) {
    emit('update:page', props.page - 1)
  }
}

const nextPage = () => {
  emit('update:page', props.page + 1)
}
</script>

<style scoped>
.pagination {
  display: flex;
  align-items: center;
  gap: 10px;
  justify-content: center;
  margin-top: 20px;
}
</style>