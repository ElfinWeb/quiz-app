<template>
  <div class="container" v-if="!quizStarted">
    <the-starter @start-quiz="startQuiz"></the-starter>
  </div>
<transition name="quiz">
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
</transition>
  <transition name="restart">
    <div class="container" v-if="quizEnded">
      <the-end
        :correctAnswers="correctAnswers"
        :count="count"
        @restart-quiz="restartQuiz"
      ></the-end>
    </div>
  </transition>
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
.restart-enter-from {
  opacity: 0;
  transform: translateY(-30px);
}
.restart-enter-active {
  transition: all 1s ease-out;
}
.restart-enter-to {
  opacity: 1;
  transform: translateY(0);
}

.quiz-enter-from {
  opacity: 0;
}
.quiz-enter-active {
  transition: all 0.3s ease-out;
}
.quiz-enter-to {
  opacity: 1;
}

.quiz-leave-from {
  opacity: 1;
}
.quiz-leave-active {
  transition: all 0.3s ease-in;
}
.quiz-leave-to {
  opacity: 0;
}
</style>
