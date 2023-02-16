<script setup>
    import {ref} from 'vue'

    defineEmits('remove-todo')

</script>

<template>
    <button @click="$emit('remove-todo')">
    <slot name="removeButton">X</slot>
    </button>
</template>