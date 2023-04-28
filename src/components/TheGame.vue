<template>
    <div id="game" class="justify-center flex-column">
        
    <h1>Quiz App</h1>
        <h2 id="quesiton">{{ questions[round].question }}</h2>
        <div :class="disabledButton">
          <div
            class="choice-container"
            v-for="(answer, index) in questions[round].options"
            :key="answer"
            @click="$emit('submit-answer', answer)"
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
          @click="$emit('next-question')"
          v-if="selectedAnswer && round < count - 1"
          >Next</a
        >
        <a
          class="btn"
          v-if="selectedAnswer && round === count - 1"
          @click="$emit('show-results')"
          >Finish</a
        >
      </div>
</template>

<script>
export default {
    props: ['questions', 'round', 'count', 'disabledButton', 'selectedAnswer'],
    emits: ['show-results', 'next-question', 'submit-answer']
}
</script>