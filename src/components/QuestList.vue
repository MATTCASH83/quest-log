<script setup>
const props = defineProps({
  quests: Array,
  selectedQuest: Object,
})

const emit = defineEmits(['quest-selected'])

const questSelected = (quest) => {
  emit('quest-selected', quest)
}

const isSelectedQuest = (quest) => {
  return props.selectedQuest && props.selectedQuest.id === quest.id
}
</script>

<template>
  <div class="wrapper">
    <div v-if="!quests.length">no quests to display...</div>
    <div
      v-for="quest in quests"
      :key="quest.id"
      @click="questSelected(quest)"
      class="quest-item"
      :class="{ selected: isSelectedQuest(quest) }"
    >
      <span v-if="isSelectedQuest(quest)">🔸</span>{{ quest.title }}
    </div>
  </div>
</template>

<style scoped>
.quest-item {
  text-align: left;
  padding: 5px;
  cursor: pointer;
}
.quest-item:hover {
  border: 0.5px rgb(250, 171, 97) solid;
  box-shadow: inset 0 5px 10px 0 rgba(250, 146, 97, 0.2);
  color: rgb(250, 171, 97);
  font-size: 14px;
  font-weight: 500;
}
.selected {
  color: rgb(250, 146, 97);
  font-size: 16px;
}
.wrapper {
  margin-bottom: 25px;
  padding-top: 10px;
  padding-bottom: 15px;
  border: 1px solid rgb(250, 146, 97);
  border-radius: 4px;
  max-height: 150px;
  overflow: auto;
}
</style>
