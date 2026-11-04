<script setup>
defineProps({
  quests: Array,
})

const emit = defineEmits(['quest-selected'])

const questSelected = (quest) => {
  emit('quest-selected', quest)
}
</script>

<template>
  <ul>
    <li v-for="quest in quests" :key="quest.id" @click="questSelected(quest)">{{ quest.title }}</li>
  </ul>
</template>

<style scoped></style>
