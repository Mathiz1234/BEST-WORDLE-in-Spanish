<script setup>
import { watch } from "vue";
import Key from './Key.vue';
const keyboardKeys = [
    ['q','w','e','r','t','y','u','i','o','p'],
    ['a','s','d','f','g','h','j','k','l','ñ'],
    ['enter','z','x','c','v','b','n','m','back']];

const props = defineProps({
    guesses: Array,
    guessIndex: Number,
    solution: String,
});

const letterInWord = {
    isInWord: [],
    isAtPosition: [],
    isNotInWord: [],
};

const emits = defineEmits(['pressed', 'letterNotInWord']);

function pressedButton(info){
    emits('pressed', info);
}

const isletterInWord = letter => {
    if(letterInWord.isNotInWord.includes(letter)) return 'notInWord';
    if(letterInWord.isAtPosition.includes(letter)) return 'atPosition';
    if(letterInWord.isInWord.includes(letter)) return 'inWord';
};

watch(
    () => props.guessIndex,
    (current, prev) => {
      props.guesses.forEach((guess, upperIndex) => {
          if(upperIndex != prev) return;
          [...guess].forEach((letter, index) => {
            if(!props.solution.includes(letter)) { letterInWord.isNotInWord.push(letter); emits('letterNotInWord', letter); return; }
            if(props.solution.charAt(index) === letter){
                letterInWord.isAtPosition.push(letter);
            }else{
                letterInWord.isInWord.push(letter);
            }
          });
      });
});

</script>

<template>
<div class="keyBoard">
    <div v-for="keyboardKey in keyboardKeys">
        <Key v-for="letter in keyboardKey" :letter="letter" @pressed="pressedButton" :isInWord="(isletterInWord(letter))"/>
    </div>
</div>
</template>

<style scoped>

.keyBoard {
  display: flex;
  flex-direction: column;
  width: 100%;
  max-width: 500px;
  min-width: 300px;
  margin: auto auto 2rem;
}

.keyBoard div {
  display: flex;
  width: 100%;
  justify-content: space-between;
  gap: 0.2rem;
  margin-bottom: 0.2rem;
}

</style>