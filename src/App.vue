<script setup>
import { ref, reactive, onMounted, computed } from "vue";
import Board from './components/Board.vue';
import KeyBoard from './components/KeyBoard.vue';

//mount dictionary
import dictionary from './dictionary';
const randomWord = computed(() => dictionary[Math.floor(Math.random() * dictionary.length + 1)]);

//game state
const state = reactive({
  solution: randomWord.value,
  guesses: ["", "", "", "", "", ""],
  currentGuessIndex: 0,
  wonGame: false,
});
const resultMsg = ref('');

//add letter to current word
const addLetter = (letter) => {
  if(state.guesses[state.currentGuessIndex].length >= 5) return;
  state.guesses[state.currentGuessIndex] += letter;
}

//delete last letter from current word
const deleteLastLetter = () => {
  if (state.guesses[state.currentGuessIndex].length === 0) return;
  state.guesses[state.currentGuessIndex] = state.guesses[state.currentGuessIndex].slice(0, -1);
}

//check if win
const checkIfWin = () =>{
    state.wonGame = state.guesses[state.currentGuessIndex-1] === state.solution;
    if(state.wonGame){
      resultMsg.value = 'You win the game!! :D';
    }else if(state.currentGuessIndex >= 6 ){
      resultMsg.value = 'You lose ;( Wanna try again?';
      state.wonGame = true;
    }
}

//validate word
const validateWord = () => {
  if(state.guesses[state.currentGuessIndex].length < 5) return;
  if(!dictionary.includes(state.guesses[state.currentGuessIndex])){
    alert("This word is not in my dictionary");
    state.guesses[state.currentGuessIndex] = '';
    return;
  }
  state.currentGuessIndex++;
  checkIfWin();
}

//disable letter actions
const disabledLetters = [];
const disableLetter = (key) => {
  disabledLetters.push(key);
}

//press key
const activateKey = (key) => {
  if(disabledLetters.includes(key) || state.wonGame) return;
  switch(key.toLowerCase()){
    case "enter":
      validateWord();
      break;
    case "back":
    case "backspace":
      deleteLastLetter();
      break;
    default:
      if(key.match(/^[a-zA-Z\u00f1\u00d1]{1}$/)) addLetter(key);
}};

//mount eventListener to user keyboard
onMounted(() => {
  document.addEventListener('keyup',({key}) => {
      activateKey(key);
  });
});

</script>

<template>
  <header>
    <h1>Wordle in Spanish</h1>
  </header>

  <main>
    <section id="app">
      <Board :guesses="state.guesses" :guessIndex="state.currentGuessIndex" :solution="state.solution"/>
      <KeyBoard @pressed="activateKey" @letterNotInWord="disableLetter" :guesses="state.guesses" :guessIndex="state.currentGuessIndex" :solution="state.solution"/>
    </section>
    <section id="result" :class="{'hidden': !resultMsg}">
        <span id="result-message"></span>
        <button onclick="location.reload();">
          {{resultMsg}}
          Click this button to play again!
        </button>
    </section>
  </main>
</template>

<style>
@import './assets/base.css';
</style>
