<script setup>
import { ref, computed } from 'vue'

const questList = ref([
  {
    id: 1,
    title: 'Rescue kitten from tree',
    difficulty: 'easy',
    xp: 100,
    completed: false,
  },
  {
    id: 2,
    title: 'Recover stolen loot from bandits',
    difficulty: 'medium',
    xp: 500,
    completed: false,
  },
  {
    id: 3,
    title: 'Defeat the dreaded dragon',
    difficulty: 'easy',
    xp: 1000,
    completed: false,
  },
])

const numberOfQuests = computed(() => {
  return questList.value.length
})
</script>

<template>
  <h1>QuestLog</h1>
  <h2>You have {{ numberOfQuests }} quests</h2>
  <ul>
    <li v-for="quest in questList" :key="quest.id">{{ quest.title }}</li>
  </ul>
</template>

<style scoped></style>
