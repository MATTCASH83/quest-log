<script setup>
import { ref, computed } from 'vue'
import AppModal from './AppModal.vue'
import '../assets/global.css'

const props = defineProps({
  existingQuest: Object,
})

const createDefaultQuest = () => ({
  id: 0,
  title: '',
  difficulty: '',
  xp: 0,
  completed: false,
})

const isEditingExistingQuest = computed(() => {
  return props.existingQuest !== null && props.existingQuest !== undefined
})

const headerText = computed(() => {
  return isEditingExistingQuest.value ? 'Edit Quest' : 'Create Quest'
})

const buttonText = computed(() => {
  return isEditingExistingQuest.value ? 'Save' : 'Create Quest'
})

const formId = computed(() => {
  return isEditingExistingQuest.value ? 'edit-request' : 'create-request'
})

const questDraft = ref(
  isEditingExistingQuest.value ? { ...props.existingQuest } : createDefaultQuest(),
)

const difficulties = ['easy', 'medium', 'hard']

const areEqual = (obj1, obj2) => {
  if (typeof obj1 !== 'object' || obj1 === null || typeof obj2 !== 'object' || obj2 === null) {
    return obj1 === obj2
  }

  const keys1 = Object.keys(obj1)
  const keys2 = Object.keys(obj2)

  if (keys1.length !== keys2.length) return false

  return keys1.every((key) => obj1[key] === obj2[key])
}

const errors = computed(() => {
  let list = []

  if (!questDraft.value.title.trim()) {
    list.push('Title cannot be blank')
  }

  if (!difficulties.includes(questDraft.value.difficulty)) {
    list.push('You must select a valid difficulty')
  }

  if (!questDraft.value.xp || questDraft.value.xp <= 0) {
    list.push('XP must be greater than 0')
  }

  if (isEditingExistingQuest.value && areEqual(questDraft.value, props.existingQuest)) {
    list.push('No changes have been made')
  }

  return list
})

const emits = defineEmits(['quest-created', 'quest-updated', 'modal-canceled'])

const handleSubmitButton = () => {
  if (isEditingExistingQuest.value) {
    emits('quest-updated', { ...questDraft.value })
    return
  }

  emits('quest-created', { ...questDraft.value })
  questDraft.value = createDefaultQuest()
}

const handleCancelButton = () => {
  emits('modal-canceled')
}
</script>

<template>
  <AppModal>
    <template #header>{{ headerText }}</template>

    <form :id="formId" class="quest-form" @submit.prevent="handleSubmitButton">
      <div class="quest-form-row">
        <label :for="`${formId}-title`">Title: </label>
        <input :id="`${formId}-title`" v-model="questDraft.title" />
      </div>
      <div class="quest-form-row">
        <label :for="`${formId}-difficulty`">Difficulty: </label>
        <select :id="`${formId}-difficulty`" v-model="questDraft.difficulty">
          <option v-for="difficulty in difficulties" :key="difficulty" :value="difficulty">
            {{ difficulty }}
          </option>
        </select>
      </div>
      <div class="quest-form-row">
        <label :for="`${formId}-xp`">XP: </label>
        <input :id="`${formId}-xp`" v-model.number="questDraft.xp" type="number" min="0" />
      </div>
      <div :class="['flex-row']">
        <button :disabled="errors.length" type="submit">
          {{ buttonText }}
        </button>
        <button type="button" @click="handleCancelButton">Cancel</button>
      </div>
      <div v-for="error in errors" :key="error" class="error">{{ error }}</div>
    </form>
  </AppModal>
</template>

<style scoped>
.quest-form {
  display: grid;
  grid-template-columns: 100%;
  align-items: center;
  justify-content: center;
  row-gap: 10px;
  width: 300px;
}
.quest-form-row {
  display: grid;
  grid-template-columns: 29% 70%;
  gap: 10px;
  align-items: center;
  justify-content: center;
  text-align: right;
}
.error {
  color: rgb(253, 71, 71);
  font-size: 14px;
  width: 100%;
  text-align: center;
}
</style>
