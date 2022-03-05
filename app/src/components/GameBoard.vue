<script setup lang="ts">
import { ref } from 'vue'
import Row from './Row.vue';
const props = defineProps<{
  rows: number,
  tiles: number,
  word: string
}>()

const rows = ref(Array.from({ length: props.rows }, (_, index) => {
  return { index: index, tiles: props.tiles }
}));

function handleGuess(guess: string): void {
  console.log(guess);
  if (guess !== props.word)
    alert('nope');
}
</script>

<template>
  <div class="gameboard">
    <h1 class="green">Vuedle!</h1>
    <Row v-for="row in rows" :tiles="row.tiles" @guess="handleGuess" />
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}
</style>
