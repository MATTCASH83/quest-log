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
  <div class="wrapper">
    <h3>Quest Details</h3>
    <div class="row">
      <div class="title">Title:</div>
      <div>{{ quest.title }}</div>
    </div>
    <div class="row">
      <div class="title">Difficulty:</div>
      <div>{{ quest.difficulty }}</div>
    </div>
    <div class="row">
      <div class="title">XP:</div>
      <div>{{ quest.xp }}</div>
    </div>
    <div class="flex-row">
      <button v-if="quest.completed === false" @click="completeQuest">Complete Quest</button>
      <button @click="handleQuestEditPressed">Edit Quest</button>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  margin-bottom: 15px;
  padding-bottom: 15px;
  border: 1px solid rgb(250, 146, 97);
  border-radius: 4px;
}
.row {
  display: grid;
  grid-template-columns: 75px 200px;
  gap: 15px;
  margin-bottom: 15px;
  align-items: center;
  justify-content: center;
  text-align: left;
  font-size: 13px;
}
.title {
  text-align: right;
  font-weight: 500;
}
</style>
