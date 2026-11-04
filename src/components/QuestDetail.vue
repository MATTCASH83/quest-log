<script setup>
defineProps({
  quest: Object,
})

const emits = defineEmits(['quest-completed'])

const completeQuest = () => {
  emits('quest-completed')
}
</script>

<template>
  <div>
    <h3>Selected Quest</h3>
    <p>{{ quest.title }}</p>
    <p>Difficulty: {{ quest.difficulty }}</p>
    <p>XP: {{ quest.xp }}</p>
    <button v-if="quest.completed === false" @click="completeQuest">Complete Quest</button>
  </div>
</template>

<style scoped></style>
