<template>
  <div class="container">
    <div id="home" class="flex-center flex-column">
      <h1>Quiz App</h1>
      <a class="btn" href="/game.html">Play</a>
      <a class="btn" href="/highscores.html">High Scores</a>
    </div>
  </div>

  <div class="container" v-if="!quizEnded">
    <div id="game" class="justify-center flex-column">
      <h2 id="quesiton">{{ questions[round].question }}</h2>
      <div ref="questionBox">
        <div
          class="choice-container"
          v-for="(answer, index) in questions[round].options"
          :key="answer"
          @click="submitAnswer"
          :class="
            {
              incorrect:
                selectedAnswer &&
                answer === selectedAnswer &&
                answer !== questions[round].options[questions[round].correct],
            },
            {
              correct:
                selectedAnswer &&
                answer === questions[round].options[questions[round].correct],
            }
          "
        >
          <p class="choice-prefix">{{ index + 1 }}</p>
          <p class="choice-text">{{ answer }}</p>
        </div>
      </div>

      <a
        class="btn"
        @click="nextQuestion"
        v-if="selectedAnswer && round < count - 1"
        >Next</a
      >
      <a
        class="btn"
        v-if="selectedAnswer && round === count - 1"
        @click="showResults"
        >Finish</a
      >
    </div>
  </div>
  <div class="container" v-if="quizEnded">
    <div class="flex-center flex-column">
      <h2>Quiz ended!</h2>
      <h3>
        Congratulation! You passed {{ correctAnswers }} of {{ count }} right!
      </h3>
      <a class="btn" v-if="quizEnded" @click="restartQuiz">Restart</a>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      quizEnded: false,
      selectedAnswer: "",
      round: 0,
      count: 3,
      correctAnswers: 0,
      questions: [
        {
          id: 0,
          question: "What is the name of the capital of Switzerland?",
          options: ["Berlin", "Paris", "Zurich", "Bern"],
          correct: 3,
        },
        {
          id: 1,
          question: "What is the language of Iranian people?",
          options: ["Arabic", "Pashto", "Persian", "Russian"],
          correct: 2,
        },
        {
          id: 2,
          question: "How many planets does the solar system have?",
          options: ["6", "11", "9", "8"],
          correct: 3,
        },
      ],
    };
  },
  watch: {
    selectedAnswer(val) {
      const correctAnswer =
        this.questions[this.round].options[this.questions[this.round].correct];
      if (val === correctAnswer) {
        this.correctAnswers++;
      }
    },
  },
  methods: {
    submitAnswer(e) {
      const answer = e.currentTarget.lastElementChild.textContent;
      this.selectedAnswer = answer;
      this.$refs.questionBox.classList.add("disabledbutton");
    },
    nextQuestion() {
      this.round++;
      this.selectedAnswer = "";
      this.$refs.questionBox.classList.remove("disabledbutton");
    },
    showResults() {
      this.quizEnded = true;
    },
    restartQuiz() {
      this.round = 0;
      this.quizEnded = false;
      this.selectedAnswer = "";
      this.correctAnswers = 0;
    },
  },
};
</script>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
