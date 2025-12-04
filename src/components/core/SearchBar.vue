<script setup lang="ts">
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faMagnifyingGlass } from '@fortawesome/free-solid-svg-icons'
import { ref } from 'vue'

const gameText = ref('')

const emit = defineEmits(['search', 'clear'])

function setGameText(e: Event) {
  gameText.value = (e.target as HTMLInputElement).value
  console.log(gameText.value)
}

function clearGameText() {
  gameText.value = ''
  emit('clear')
}

function submitSearch() {
  emit('search', gameText.value)
}
</script>

<template>
  <form v-on:submit.prevent="submitSearch" v-on:reset="clearGameText">
    <FontAwesomeIcon :icon="faMagnifyingGlass" />
    <input
      :value="gameText"
      v-on:input="setGameText"
      type="text"
      name="game"
      id="game"
      placeholder="Enter game title..."
    />
    <button type="submit" class="btn-submit">Search</button>
    <button type="reset" class="btn-clear">Clear</button>
  </form>
</template>

<style lang="css" scoped>
.glass {
  justify-content: start;
  padding: 16px;
  margin-top: 8px;
  gap: 4px;
}

#game {
  width: 100%;
}
</style>
