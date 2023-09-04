<template>
  <div id="app">
    <h1>Super Quiz</h1>
    <transition name="flip" mode="out-in">
      <Question v-if="questionMode"
        :question="questions[currentQuestion]"
        @answered="showResult" />
      <Result v-else
        :result="result"
        @confirmed="nextQuestion" />
    </transition>

    <div class="square">
      <p>Respostas Corretas: {{ correctAnswers }}</p>
      <button @click="restartGame">Reiniciar Jogo</button>
    </div>
  </div>
</template>

<script>
import questions from './util/questions'
import Question from './components/Question.vue'
import Result from './components/Result.vue'

export default {
	name: 'app',
	components: { Question, Result },
	data() {
		return {
			result: false,
			questionMode: true,
			questions,
			currentQuestion: 0,
      correctAnswers: 0
		}
	},
	methods: {
		showResult(result) {
			this.result = result;
			this.questionMode = false;

			if (result === true) {
				this.correctAnswers++;
				if (this.correctAnswers === 5) {
					alert('Você ganhou! O jogo acabou.');
					this.restartGame();
				}
			} else {
				this.correctAnswers = 0;
			}
		},
		nextQuestion() {
			this.questionMode = true
			let r = Math.random() * this.questions.length
			this.currentQuestion = parseInt(r)
		},
		restartGame() {
			this.result = false;
			this.questionMode = true;
			this.currentQuestion = 0;
			this.correctAnswers = 0;
 		}
	}
}
</script>

<style>
body {
	background: linear-gradient(to right, rgb(0, 0, 70), rgb(28, 181, 224));
	font-family: 'Oswald', sans-serif;
	color: #FFF;
	height: 100vh;
}

#app {
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;

	display: flex;
	flex-direction: column;
	align-items: center;
}

#app h1 {
	font-weight: 200;
	font-size: 4rem;
}

@keyframes flip-out {
	from { transform: rotateY(0deg); }
	to { transform: rotateY(90deg); }
}

@keyframes flip-in {
	from { transform: rotateY(90deg); }
	to { transform: rotateY(0deg); }
}

.flip-enter-active {
	animation: flip-in 0.3s ease;
}

.flip-leave-active {
	animation: flip-out 0.3s ease;
}

.square button {
  width: 100px;
  height: 60px; /* Altura desejada para torná-lo quadrado */
  background-color: black;
  color: white;
  border: none;
  cursor: pointer;
  font-size: 20px;
	border-radius: 10px;
}

</style>
