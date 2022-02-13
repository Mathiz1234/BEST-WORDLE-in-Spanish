<script setup>
import Frame from './Frame.vue';

const props = defineProps({
    guesses: Array,
    guessIndex: Number,
    solution: String,
});

</script>

<template>
<div class="board">
    <div v-for="(guess, index) in guesses">
        <Frame v-for="n in 5" :letter="guess[n-1]" :positon="n-1" :line="index+1" :guessIndex="guessIndex" :solution="solution"/>
    </div>
</div>
</template>

<style scoped>

.board {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  max-width: 800px;
  min-width: 250px;
  margin: 2rem auto;
}

.board div {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 0.5rem;
}

</style>