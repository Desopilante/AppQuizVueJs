<template>
	<b-jumbotron>
		<template v-slot:lead>
			{{currentQuestion.question}}
		</template>
		<hr class="my-4">
		<b-list-group>
			<b-list-group-item button v-for="(answer,index) in answers" :key="index" @click="selectAnswer(index)" :class="answerClass(index)">{{answer}}</b-list-group-item>
		</b-list-group>
		<b-button variant="primary" href="#" @click="submitAnswer" :disabled="selectedIndex===null || answered === true">Submit</b-button>
		<b-button @click="next" variant="success" href="#">Next</b-button>
	</b-jumbotron>
</template>

<script>
	import * as shuffleJs from '../js/shuffle.js'

	export default {
		props: {
			currentQuestion: Object,
			next: Function,
			increment: Function,
			numTotal: Number
		},
		watch: {
			currentQuestion: {
				immediate: true,
				handler() {
					this.selectedIndex = null
					this.answered = false
				}
			}
		},
		data() {
			return {
				selectedIndex: null,
				correctIndex: null,
				answered: false
			}
		},
		methods: {
			selectAnswer(index){
				this.selectedIndex = index
			},
			submitAnswer(){
				this.correctIndex = this.answers.indexOf(this.currentQuestion.correct_answer)
				let isCorrect = false
				if (this.selectedIndex === this.correctIndex){
					isCorrect = true
				}
				this.answered = true
				this.increment(isCorrect)
			},
			answerClass(index){
				let answerClass = ''
				if (!this.answered && this.selectedIndex === index) {
					answerClass = 'active'
				}else{
					if(this.answered && this.correctIndex === index){
						answerClass = 'correct'
					}else{
						if(this.answered && this.selectedIndex === index && this.correctIndex !== index){
							answerClass = 'incorrect'
						}
					}
				}
				return answerClass
			}
		},
		computed: {
			answers() {
				let answers = [...this.currentQuestion.incorrect_answers]
				answers.push(this.currentQuestion.correct_answer)
				answers = shuffleJs.shuffle(answers)
				return answers
			}
		}
	}
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.btn {
  margin: 0 5px;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: red;
}
</style>