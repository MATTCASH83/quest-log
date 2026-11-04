<script setup>
import { ref, computed } from 'vue'
import QuestList from './components/QuestList.vue'
import QuestDetail from './components/QuestDetail.vue'

const quests = ref([
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

const filterValue = ref('all')

const filteredQuests = computed(() => {
  if (filterValue.value === 'active') {
    return quests.value.filter((x) => x.completed === false)
  }

  if (filterValue.value === 'completed') {
    return quests.value.filter((x) => x.completed === true)
  }

  return quests.value
})

const numberOfQuests = computed(() => {
  return quests.value.length
})

const numberOfCompletedQuests = computed(() => {
  return quests.value.filter((x) => x.completed === true).length
})

const remainingXp = computed(() => {
  return quests.value.filter((x) => x.completed === false).reduce((sum, x) => sum + x.xp, 0)
})

const selectedQuest = ref(null)

const handleQuestSelected = (quest) => {
  selectedQuest.value = quest
}

const handleQuestCompleted = () => {
  selectedQuest.value.completed = true

  if (filterValue.value === 'active') {
    selectedQuest.value = null
  }
}

const handleFilterSelected = (value) => {
  if (filterValue.value === value) {
    return
  }
  filterValue.value = value
  selectedQuest.value = null
}
</script>

<template>
  <h1>QuestLog</h1>
  <p>You have {{ numberOfQuests }} quests</p>
  <p>{{ numberOfCompletedQuests }} completed</p>
  <p>{{ remainingXp }} total XP available</p>
  <button @click="handleFilterSelected('all')" :disabled="filterValue === 'all'">All</button>
  <button @click="handleFilterSelected('active')" :disabled="filterValue === 'active'">
    Active
  </button>
  <button @click="handleFilterSelected('completed')" :disabled="filterValue === 'completed'">
    Completed
  </button>
  <QuestList :quests="filteredQuests" @quest-selected="handleQuestSelected" />
  <QuestDetail
    v-if="selectedQuest !== null"
    :quest="selectedQuest"
    @quest-completed="handleQuestCompleted"
  />
</template>

<style scoped></style>
