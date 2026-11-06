<script setup>
import { ref, computed, watch } from 'vue'
import QuestList from './components/QuestList.vue'
import QuestDetail from './components/QuestDetail.vue'
import QuestForm from './components/QuestForm.vue'
import './assets/global.css'

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
const searchText = ref('')
const appState = ref('view')

const filteredQuests = computed(() => {
  let list = quests.value

  if (searchText.value) {
    list = list.filter((x) => x.title.toLowerCase().includes(searchText.value.toLowerCase()))
  }

  if (filterValue.value === 'active') {
    return list.filter((x) => x.completed === false)
  }

  if (filterValue.value === 'completed') {
    return list.filter((x) => x.completed === true)
  }

  return list
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
  appState.value = 'view'
}

const handleQuestCompleted = () => {
  selectedQuest.value.completed = true
}

const handleModalCanceled = () => {
  appState.value = 'view'
}

const handleFilterSelected = (value) => {
  if (filterValue.value === value) {
    return
  }
  filterValue.value = value
}

watch(filteredQuests, () => {
  if (selectedQuest.value && !filteredQuests.value.find((x) => x.id === selectedQuest.value.id)) {
    selectedQuest.value = null
  }
})

const handleQuestEditPressed = () => {
  appState.value = 'edit'
}

const handleCreatePressed = () => {
  appState.value = 'create'
}

const handleQuestUpdated = (quest) => {
  selectedQuest.value.title = quest.title
  selectedQuest.value.difficulty = quest.difficulty
  selectedQuest.value.xp = quest.xp

  appState.value = 'view'
}

const handleQuestCreated = (quest) => {
  let newId =
    quests.value.reduce(
      (max, current) => {
        return current.id > max.id ? current : max
      },
      { id: 0 },
    ).id + 1

  quest.id = newId

  quests.value.push(quest)

  appState.value = 'view'
}
</script>

<template>
  <div class="wrapper">
    <h1>QuestLog</h1>
    <p>You have {{ numberOfQuests }} quests</p>
    <p>{{ numberOfCompletedQuests }} completed</p>
    <p>{{ remainingXp }} total XP available</p>

    <div class="flex-row">
      <button class="button-wide" @click="handleCreatePressed">Create Quest</button>
    </div>

    <div class="quest-filters">
      <div>
        <label>Search: </label>
        <input type="text" v-model="searchText" />
      </div>

      <div class="flex-row">
        <button @click="handleFilterSelected('all')" :disabled="filterValue === 'all'">All</button>

        <button @click="handleFilterSelected('active')" :disabled="filterValue === 'active'">
          Active
        </button>

        <button @click="handleFilterSelected('completed')" :disabled="filterValue === 'completed'">
          Completed
        </button>
      </div>
    </div>
  </div>

  <QuestList
    :quests="filteredQuests"
    :selected-quest="selectedQuest"
    @quest-selected="handleQuestSelected"
  />

  <QuestDetail
    v-if="selectedQuest !== null"
    :quest="selectedQuest"
    @quest-completed="handleQuestCompleted"
    @edit-pressed="handleQuestEditPressed"
  />

  <QuestForm
    v-if="appState === 'edit'"
    @quest-updated="handleQuestUpdated"
    @modal-canceled="handleModalCanceled"
    :existingQuest="selectedQuest"
  />

  <QuestForm
    v-if="appState === 'create'"
    @quest-created="handleQuestCreated"
    @modal-canceled="handleModalCanceled"
  />
</template>

<style scoped>
.button-wide {
  width: 250px;
}
.quest-filters {
  margin-top: 25px;
  margin-bottom: 25px;
}
.wrapper {
  min-width: 300px;
}
</style>
