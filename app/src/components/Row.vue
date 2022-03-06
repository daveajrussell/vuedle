<script setup lang="ts">
import { reactive, watch, ref, onUpdated } from 'vue'
import Tile from './Tile.vue';

const props = defineProps<{
    rowIndex: number,
    tiles: number,
    enabled: boolean,
    word: string
}>()

const emit = defineEmits(['guess']);

const tiles = reactive(Array.from({ length: props.tiles }, (_, index) => {
    return {
        index: index,
        value: '',
        enabled: props.enabled && index === 0,
        focus: props.enabled && index === 0,
        correctLetter: false,
        correctPosition: false
    }
}));

let guess = ref(''),
    guessed = ref(false);

watch(tiles, async (_, __) => {
    guess.value = tiles.map(tile => tile.value).join('');
    const currentIndex = tiles.findIndex(x => x.enabled);
    if (currentIndex >= 0 && currentIndex < props.tiles - 1) {
        const nextTile = tiles[currentIndex + 1],
            currentTile = tiles[currentIndex];
        if (currentTile.value) {
            currentTile.enabled = currentTile.focus = false
            nextTile.enabled = nextTile.focus = true;
        }
    } else if (currentIndex === props.tiles - 1) {
        const currentTile = tiles[currentIndex];
        if (currentTile.value) {
            currentTile.focus = false;
        }
    }
});

onUpdated(() => {
    if (props.enabled && tiles.every(tile => !tile.enabled)) {
        tiles[0].enabled = tiles[0].focus = true;
    }
});

function guessWord(): void {
    const currentIndex = tiles.findIndex(x => x.enabled), currentTile = tiles[currentIndex];
    if (currentIndex === props.tiles - 1) {
        guessed.value = true;
        const word = props.word.split('');
        const attempts = word.map((e) => { return { letter: e, guessed: false } });

        tiles.forEach((tile, index) => {
            tile.enabled = tile.focus = false;
            const value = tile.value;
            const occurrences = word.reduce((a: any[], e: string, i: number) => {
                if (e === value)
                    a.push(i);
                return a;
            }, []);

            if (occurrences.length > 0) {
                const attempt = attempts.find(e => e.letter === value && !e.guessed);
                if (attempt) {
                    attempt.guessed = true;
                    tile.correctLetter = true;
                    tile.correctPosition = occurrences.some((e) => e === index);
                }
            }
        });

        emit('guess', guess);
    }
}

function handleDelete(): void {
    const currentIndex = tiles.findIndex(x => x.enabled), currentTile = tiles[currentIndex];
    if (currentIndex === props.tiles - 1 && currentTile.value !== '') {
        // noop
    } else if (currentIndex > 0) {
        const previousTile = tiles[currentIndex - 1];
        currentTile.enabled = currentTile.focus = false
        previousTile.enabled = previousTile.focus = true;
        previousTile.value = '';
    }
}
</script>

<template>
    <div class="row">
        <Tile
            v-for="tile in tiles"
            v-model="tile.value"
            :enabled="tile.enabled"
            :focus="tile.focus"
            :correct-letter="tile.correctLetter"
            :correct-position="tile.correctPosition"
            @backspace="handleDelete()"
            @enter="guessWord()"
        />
    </div>
</template>

<style scoped>
.row {
    margin-bottom: 4px;
    display: flex;
    flex-shrink: 1;
}
</style>