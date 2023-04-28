<template>
<div class="container">
    <div class="flex-center flex-column">
        <h2>Quiz ended!</h2>
        <h3>
          Congratulation! You passed {{ correctAnswers }} of {{ count }} right!
        </h3>
        <a class="btn" @click="$emit('restart-quiz')">Restart</a>
      </div>
</div>
</template>

<script>
export default {
    props: ['correctAnswers', 'count']
}
</script>