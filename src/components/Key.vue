<script setup>
import { ref, watch } from "vue";
const props = defineProps({
    letter: String,
    isInWord: String,
});

const emits = defineEmits(['pressed']);

function pressedButton(){
    emits('pressed', props.letter);
}

</script>

<template>
<button :class="{
  'wrong': props.isInWord === 'notInWord',
  'warning': props.isInWord === 'inWord',
  'success': props.isInWord === 'atPosition',
}" @click="pressedButton">{{props.letter}}</button>
</template>

<style scoped>

button {
  border: 0;
  font-size: .75rem;
  border-radius: 0.4rem;
  padding: 10px 6px;
  min-width: 2rem;
  background-color: lightgray;
  cursor: pointer;
  transition: background-color 400ms;
  font-size: 1rem;
  flex-grow: 1;
}
button:hover {
  background-color: white;
  outline: 2px lightgray solid;
  color: black;
}
.success {
  background-color: green;
  color: white;
}
.warning {
  background-color: gold;
  color: white;
}
.wrong {
  background-color: gray;
  color: white;
  pointer-events: none;
}

</style>