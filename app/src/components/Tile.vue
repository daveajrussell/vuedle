<script setup lang="ts">
defineProps(['modelValue']);
defineEmits(['update:modelValue']);

function onKeyPress(e: any): boolean {
    const key = e.key.toLowerCase();
    if (key >= 'a' && key <= 'z') {
        return true;
    }
    e.preventDefault();
    return false;
}
</script>

<template>
    <input
        type="text"
        maxlength="1"
        :value="modelValue"
        @keypress="(e) => onKeyPress(e)"
        @input="$emit('update:modelValue', $event.target.value)"
    />
</template>

<style scoped>
input {
    width: 50px;
    height: 50px;
    border: 2px solid;
    font-size: large;
    text-transform: uppercase;
    text-align: center;
    background-color: var(--color-background);
    color: var(--color-border);
}

input:focus {
    outline: none;
}
</style>