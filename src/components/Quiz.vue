<template>
    <div v-if="!completedQuiz">
        <h2>{{ props.quiz.title }}</h2>
        <ProgressBar :currentValue="currentQuestionIndex" :maximum="props.quiz.questions.length" />
        <Question :question="currentQuestion" @answer="answerQuestion" />
    </div>

    <div v-else>
        <h3>{{ playerWon ? props.quiz.success_message : props.quiz.failure_message }}</h3>
        Votre score est de: {{ playerScore }} points   
    </div>
</template>

<script setup>
import { computed, ref, watch } from 'vue';
import ProgressBar from './ProgressBar.vue';
import Question from './Question.vue';

const props = defineProps({
    quiz: Object
})

const currentQuestionIndex = ref(0)
const currentQuestion = computed(() => props.quiz.questions[currentQuestionIndex.value])
const answers = ref(props.quiz.questions.map(() => null))
const completedQuiz = ref(false)
const playerScore = ref(0)
const playerWon = computed(() => playerScore.value >= props.quiz.minimum_score)

const answerQuestion = (selectedAnswer) => {
    answers.value[currentQuestionIndex.value] = selectedAnswer

    if ((currentQuestionIndex.value + 1) < props.quiz.questions.length) {
        currentQuestionIndex.value++
    }
    else {
        completedQuiz.value = true
    }
}

watch(() => completedQuiz.value, () => {
    for (let i = 0; i < props.quiz.questions.length; i++) {
        if (props.quiz.questions[i].correct_answer === answers.value[i]) {
            playerScore.value++
        }
    }
})
</script>

<style scoped>
    .loading {
        display: flex;
        justify-content: center;
    }
</style>