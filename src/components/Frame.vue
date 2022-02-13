<script setup>
import { watch, ref } from "vue";

const props = defineProps({
    letter: String,
    line: Number,
    guessIndex: Number,
    solution: String,
    positon: Number,
});

const isInWord = ref('no');

watch(
    () => props.guessIndex,
    (current, prev) => {
      if(current != props.line) return;
      if(!props.solution.includes(props.letter)) return;
      isInWord.value = (props.solution.charAt(props.positon) === props.letter) ? 'at' : 'yes';
});

</script>

<template>
<span class="frame" :class="{
  'warning': isInWord === 'yes',
  'success': isInWord === 'at',
}">{{letter?.toUpperCase()}}</span>
</template>

<style scoped>

.frame {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  font-size: 2rem;
  font-weight: 800;
  text-align: center;
  margin: auto;
  width: 100%;
  aspect-ratio: 1;
  outline: 1px solid gray;
}

.success {
  background-color: green;
  color: white;
}
.warning {
  background-color: gold;
  color: white;
}

</style>