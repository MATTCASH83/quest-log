<script setup>
import { ref, computed } from 'vue'

const isNumericOnly = (text) => /^\d+$/.test(text)

const createDefaultQuest = () => ({
  id: 0,
  title: '',
  difficulty: '',
  xp: 0,
  completed: false,
})

const questDraft = ref(createDefaultQuest())

const difficulties = ref(['easy', 'medium', 'hard'])

const errors = computed(() => {
  let list = []

  if (!questDraft.value.title.trim()) {
    list.push('Title cannot be blank or all whitespaces')
  }

  if (!questDraft.value.xp || !isNumericOnly(questDraft.value.xp) || questDraft.value.xp <= 0) {
    list.push('XP must be a number greater than 0')
  }

  if (
    !questDraft.value.difficulty.trim() ||
    !difficulties.value.includes(questDraft.value.difficulty)
  ) {
    list.push('You must select a valid difficulty')
  }

  return list
})

const emits = defineEmits(['quest-created'])

const createQuest = () => {
  emits('quest-created', { ...questDraft.value })

  questDraft.value = createDefaultQuest()
}
</script>

<template>
  <form class="quest-form" @submit.prevent>
    <h3>Create Quest</h3>
    <div class="quest-form-row">
      <label for="title">Title: </label>
      <input id="title" v-model="questDraft.title" />
    </div>
    <div class="quest-form-row">
      <label for="difficulty">Difficulty: </label>
      <select id="difficulty" v-model="questDraft.difficulty">
        <option v-for="difficulty in difficulties" :key="difficulty" :value="difficulty">
          {{ difficulty }}
        </option>
      </select>
    </div>
    <div class="quest-form-row">
      <label for="xp">XP: </label>
      <input id="xp" v-model.number="questDraft.xp" type="number" min="0" />
    </div>
    <button :disabled="errors.length" type="submit" @click="createQuest">Create Quest</button>
    <div v-for="error in errors" :key="error" style="color: red">{{ error }}</div>
  </form>
</template>

<style scoped>
.quest-form {
  display: grid;
  align-items: center;
  justify-content: center;
  width: 275px;
  row-gap: 10px;
}
.quest-form-row {
  display: grid;
  grid-template-columns: 29% 70%;
  gap: 5px;
  align-items: center;
  justify-content: center;
  text-align: right;
  width: 275px;
}
</style>
