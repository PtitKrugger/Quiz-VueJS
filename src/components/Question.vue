<template>
    <fieldset>
        <legend>{{ question.question }}</legend>
        <Answer v-for="(choice, index) in randomChoices"
                    :id="`answer-${index}`"
                    :disabled="answerChecked"
                    :key="index" 
                    :value="choice" 
                    v-model:selectedAnswer="selectedAnswer"
                    :correctAnswer="question.correct_answer">{{ choice }}</Answer>
    </fieldset>
    <div class="button-container">
        <button :disabled="!selectedAnswer || answerChecked" @click="checkAnswer">Valider</button>
        <button :disabled="!selectedAnswer || !answerChecked" @click="emits('answer', selectedAnswer)" class="secondary">Question suivante</button>        
    </div>
</template>

<script setup>
import { computed, ref, watch } from 'vue';
import { shuffleArray } from '@/functions/Array';
import Answer from './Answer.vue';

const props = defineProps({
    question: Object
})
const emits = defineEmits(['answer'])

const selectedAnswer = ref(null)
const answerChecked = ref(false)
const checkAnswer = () => answerChecked.value = !answerChecked.value
const randomChoices = computed(() => shuffleArray(props.question.choices))

watch(() => props.question, () => {
    selectedAnswer.value = null
    answerChecked.value = null
})
</script>

<style>
.button-container {
    display: flex;
    justify-content: space-between;
    width: 100%;          
}
</style>