<script setup lang="ts">
import { reactive, ref } from 'vue'
import Row from './Row.vue';
const props = defineProps<{
  rows: number,
  tiles: number,
  word: string
}>()

const rows = reactive(Array.from({ length: props.rows }, (_, index) => {
  return { index: index, tiles: props.tiles, enabled: index === 0 }
}));

let win = ref(false), lose = ref(false), currentRow = ref(0);

function handleGuess(guess: any): void {
  if (guess.value !== props.word && currentRow.value < props.rows - 1) {
    rows[currentRow.value].enabled = false;
    rows[currentRow.value + 1].enabled = true;
    currentRow.value += 1;
  } else if (guess.value === props.word) {
    win.value = true;
  } else {
    lose.value = true;
  }
}
</script>

<template>
  <div class="gameboard">
    <h1 class="green">Vuedle!</h1>
    <span class="green" v-if="lose">
      <h2>Better luck next time!</h2>
      <h2>
        The word was "<b>{{ word }}</b>"
      </h2>
    </span>
    <h2 class="green" v-if="win">You correctly guessed {{ word }} in {{ currentRow + 1 }} {{ currentRow === 0 ? 'guess' : 'guesses' }}!</h2>
    <div class="rows">
      <Row
        v-for="(row, index) in rows"
        :row-index="index"
        :tiles="row.tiles"
        :enabled="row.enabled"
        :word="word"
        @guess="handleGuess"/>
    </div>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
  color: var(--green);
}
h2 {
  font-weight: 500;
  font-size: 1.2rem;
  color: var(--green);
}

.gameboard {
  margin: 0 24px 0 24px;
}

.rows {
  margin: 24px 0 24px 0;
}
</style>
