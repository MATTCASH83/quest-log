<script setup>
import '../assets/global.css'

defineProps({
  quest: Object,
})

const emits = defineEmits(['quest-completed', 'edit-pressed'])

const completeQuest = () => {
  emits('quest-completed')
}

const handleQuestEditPressed = () => {
  emits('edit-pressed')
}
</script>

<template>
  <div>
    <h3>Selected Quest</h3>
    <p>{{ quest.title }}</p>
    <p>Difficulty: {{ quest.difficulty }}</p>
    <p>XP: {{ quest.xp }}</p>
    <div class="flex-row">
      <button v-if="quest.completed === false" @click="completeQuest">Complete Quest</button>
      <button @click="handleQuestEditPressed">Edit Quest</button>
    </div>
  </div>
</template>

<style scoped></style>
