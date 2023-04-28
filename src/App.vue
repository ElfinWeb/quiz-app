<template>
  <div class="container" v-if="!quizStarted">
    <the-starter @start-quiz="startQuiz"></the-starter>
  </div>

  <div class="container" v-if="!quizEnded && quizStarted">
    <the-game
      :questions="questions"
      :round="round"
      :count="count"
      :setClass="disableClass ? 'disabledButton' : ''"
      :selectedAnswer="selectedAnswer"
      @show-results="showResults"
      @next-question="nextQuestion"
      @submit-answer="submitAnswer"
    ></the-game>
  </div>
  <div class="container" v-if="quizEnded">
    <the-end
      :correctAnswers="correctAnswers"
      :count="count"
      @restart-quiz="restartQuiz"
    ></the-end>
  </div>
</template>

<script>
import TheEnd from "./components/TheEnd.vue";
import TheGame from "./components/TheGame.vue";
import TheStarter from "./components/TheStarter.vue";
export default {
  components: {
    TheEnd,
    TheGame,
    TheStarter,
  },
  data() {
    return {
      quizStarted: false,
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
  computed: {
    disableClass() {
      return this.selectedAnswer === "";
    },
  },
  methods: {
    startQuiz() {
      this.quizStarted = true;
    },
    submitAnswer(answer) {
      this.selectedAnswer = answer;
    },
    nextQuestion() {
      this.round++;
      this.selectedAnswer = "";
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
