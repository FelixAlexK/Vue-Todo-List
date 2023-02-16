<script setup>
    import { ref } from 'vue';

    defineProps(['modelValue', 'priorityValue'])

    defineEmits(['update:modelValue', 'add-todo', 'update:priorityValue'])
  
</script>

<template>
    <form class="add-item-form" @submit.prevent="$emit('add-todo')">
        <input :value="modelValue" @input="$emit('update:modelValue', $event.target.value)"/>
        <label>
            <slot name="checkbox">High Piority</slot>
        </label>
        <button :disabled="modelValue.length < 1" class="btn btn-primary" ><slot name="addButton">Add Button</slot></button>
    </form>
</template>
