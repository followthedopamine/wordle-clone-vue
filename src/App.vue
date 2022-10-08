<script setup>
import GridBox from "./components/GridBox.vue";
import { ref } from "vue";

const createGrid = ([rows, cols]) => {
  const g = new Array(rows);
  for (let i = 0; i < g.length; i++) {
    g[i] = new Array(cols).fill(" ");
  }
  return g;
};

const inputCharToGrid = (char) => {
  if (currentCol < wordLength) {
    grid[currentRow][currentCol] = char;
    currentCol++;
  }
};

const handleKeyboard = (event) => {
  console.log(event.key);
  inputCharToGrid(event.key);
  forceRerender();
};

const componentKey = ref(0);

const forceRerender = () => {
  componentKey.value += 1;
};

const wordLength = 5;
const numberOfGuesses = 6;
const gridSize = [numberOfGuesses, wordLength];
const grid = createGrid(gridSize);
let currentRow = 0;
let currentCol = 0;
document.body.addEventListener("keydown", handleKeyboard);
</script>

<template>
  <div class="container" :key="componentKey">
    <div>
      <div
        class="row"
        :class="'row' + index"
        v-for="(row, index) in grid"
        :key="'row' + index"
      >
        <GridBox
          v-for="(col, index) in row"
          :key="'col' + index"
          :value="col"
        />
      </div>
    </div>
  </div>
</template>

<style>
:root {
  --green: #6aaa64;
  --yellow: #c9b458;
  --dark-grey: #333;
  --light-grey: #555;
}

body {
  background-color: #222;
  color: white;
  font-size: 30px;
  font-family: "Clear Sans", "Helvetica Neue", Arial, sans-serif;
  font-weight: bold;
}
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.row {
  display: flex;
}
</style>
