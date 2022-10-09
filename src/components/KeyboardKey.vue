<script setup>
import { defineProps } from "vue";
const cLevel = require("../utils/cLevel");

const props = defineProps({
  data: Object,
});

const extraClass = props.data.key === "enter" ? "enter-key" : "";

let className = "";
if (props.data.correctness === cLevel.correct) {
  className = "green";
}
if (props.data.correctness === cLevel.yellow) {
  className = "yellow";
}
if (props.data.correctness === cLevel.incorrect) {
  className = "dark-grey";
}
</script>

<template>
  <button class="keyboard-key" :class="extraClass + ' ' + className">
    {{ props.data.key.toUpperCase() }}
  </button>
</template>

<style scoped>
.keyboard-key {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 35px;
  height: 60px;
  border: none;
  border-radius: 4px;
  background-color: #555;
  margin: 3px;
  font-family: "Clear Sans", "Helvetica Neue", Arial, sans-serif;
  font-weight: bold;
  font-size: 16px;
  color: white;
}

.enter-key {
  width: 80px;
}

/* Not sure why I need these here */
.green {
  background-color: var(--green);
  border-color: var(--green);
}

.yellow {
  background-color: var(--yellow);
  border-color: var(--yellow);
}

.dark-grey {
  background-color: var(--dark-grey);
  border-color: var(--dark-grey);
}

.light-grey {
  background-color: var(--light-grey);
  border-color: var(--light-grey);
}
</style>
