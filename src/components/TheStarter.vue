<template>
  <div id="home" class="flex-center flex-column">
    <h1>Quiz App</h1>
    <a class="btn" @click="$emit('start-quiz')">Play</a>
  </div>
</template>

<script>
export default {
  emits: ["start-quiz"],
};
</script>
