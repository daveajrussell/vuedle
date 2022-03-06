<script setup lang="ts">
import { ref, onUpdated } from 'vue';
const props = defineProps(['modelValue', 'enabled', 'focus', 'correctLetter', 'correctPosition']);
const emits = defineEmits(['update:modelValue', 'backspace']);
const input = ref(null);

function onKeyPress(e: any): boolean {
    const key = e.key.toLowerCase();
    if (key >= 'a' && key <= 'z') {
        return true;
    }
    e.preventDefault();
    return false;
}

function onKeyDown(e: any) {
    if (e.keyCode === 8) {
        emits('backspace');
    }
}

onUpdated(() => {
    if (props.focus) {
        input.value.focus();
    }
})
</script>

<template>
    <input
        ref="input"
        :disabled="!props.enabled"
        type="text"
        maxlength="1"
        :value="modelValue"
        :class="props.correctPosition ? 'correct-position' : props.correctLetter ? 'correct-letter' : ''"
        @keypress="(e) => onKeyPress(e)"
        @keydown="(e) => onKeyDown(e)"
        @input="emits('update:modelValue', $event.target.value)"
    />
</template>

<style scoped>
input {
    width: 50px;
    height: 50px;
    margin-left: -2px;
    border: 2px solid var(--color-text);
    font-size: 1rem;
    text-transform: uppercase;
    text-align: center;
    background-color: var(--color-background);
    color: var(--color-text);
    font-weight: 900;
}

input:focus {
    outline: none;
}

input.correct-letter {
    background-color: var(--ochre);
    color: var(--color-background);
}

input.correct-position {
    background-color: var(--green);
    color: var(--color-background);
}
</style>