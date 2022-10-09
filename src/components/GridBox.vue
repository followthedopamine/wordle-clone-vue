<script setup>
import { defineProps } from "vue";
const cLevel = require("../utils/cLevel");

const props = defineProps({
  data: Object,
});

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
if (props.data.correctness === cLevel.error) {
  className = "error";
}
</script>

<template>
  <span :class="className">{{ props.data.char.toUpperCase() }} </span>
</template>

<style scoped>
span {
  display: flex;
  width: 50px;
  height: 50px;
  margin: 4px;
  justify-content: center;
  align-items: center;
  border: 2px solid var(--dark-grey);
}

.error {
  border-color: #ad2a2a;
}
</style>
