<script setup lang="ts">
import GameBoard from './components/GameBoard.vue';
import { onMounted, ref } from 'vue';

let word = ref('');

onMounted(() => {
  fetch('http://vuedle.daveajrussell.com/common-7-letter-words.txt')
    .then(response => response.text())
    .then((words) => {
      const availableWords = words.split('\n'),
        index = Math.floor(Math.random() * availableWords.length);
      word.value = availableWords[index].toLowerCase();
    });
});
</script>

<template>
  <main>
    <GameBoard :rows="8" :tiles="7" :word="word" />
  </main>
</template>

<style>
@import "./assets/base.css";

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  font-weight: normal;
}

@media (min-width: 1024px) {
  body {
    display: flex;
    place-items: center;
  }
}
</style>
