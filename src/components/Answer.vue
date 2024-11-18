<template>
    <label :class="classes">
        <input type="radio" name="question" :id="id" v-model="model" :value="value" :disabled="disabled">
        <slot></slot>
    </label>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
    id: String,
    disabled: Boolean,
    value: String,
    correctAnswer: String
})

const model = defineModel('selectedAnswer', String)
const classes = computed(() => ({
    disabled: props.disabled,
    right: props.disabled && props.value === props.correctAnswer,
    wrong: props.disabled && props.value !== props.correctAnswer && model.value === props.value
}))
</script>

<style scoped>
    .disabled {
        pointer-events: none;
    }
    .right {
        color: green;
    }
    .wrong {
        color: red;
    }
</style>