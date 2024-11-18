<template>
	<main class="container">
		<div v-if="state === 'loading'" class="loading" >
			<button aria-busy="true" aria-label="Please wait…">Chargement des données</button>
		</div>  
		
		<div v-else-if="state === 'error'">
			<h2>Impossible de récupérer les données</h2>
		</div>

		<template v-else>
			<Quiz :quiz="data" />
		</template>
	</main>		
</template>

<script setup>
import { onMounted, ref } from 'vue';
import Quiz from './components/Quiz.vue';

const data = ref(null)
const state = ref('loading')

const fetchData = () => {
	fetch('/quiz.json')
        .then(r => {
            if (r.ok) {
                return r.json()
            }
            state.value = 'error'
            throw new Error('Impossible de récupérer les données')
        })
        .then(r => {
            data.value = r
            state.value = 'idle'
        })
        .catch(e => {
            state.value = 'error'
            console.log(e)
        });
}

onMounted(() => {
	fetchData();
	document.querySelector('html').setAttribute('data-theme', 'light');
})
</script>

<style scoped>
	.container {
		width: 50vw;
	}
</style>