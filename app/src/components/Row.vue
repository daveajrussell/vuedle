<script setup lang="ts">
import { ref } from 'vue'
import Tile from './Tile.vue';

const props = defineProps<{
    tiles: number
}>()

const emit = defineEmits(['guess']);

const tiles = ref(Array.from({ length: props.tiles }, (_, index) => {
    return { index: index, value: '' }
}));

function guess(): void {
    const guess = tiles.value.map(tiles => tiles.value).join('');
    emit('guess', guess);
}
</script>

<template>
    <div class="row">
        <Tile v-for="tile in tiles" v-model="tile.value" />
        <button @click="guess()">Guess</button>
    </div>
</template>

<style scoped>
button {
    display: inline-block;
    border: none;
    padding: 1rem 2rem;
    margin: 0;
    text-decoration: none;
    background: var(--color-background);
    border: 2px solid var(--color-border);
    color: #ffffff;
    font-family: sans-serif;
    font-size: 1rem;
    cursor: pointer;
    text-align: center;
    transition: background 250ms ease-in-out, transform 150ms ease;
    -webkit-appearance: none;
    -moz-appearance: none;
}

button:hover,
button:focus {
    background: var(--color-border);
}

button:focus {
    outline: 2px solid var(--color-border);
    outline-offset: -4px;
}

button:active {
    transform: scale(0.99);
}
</style>