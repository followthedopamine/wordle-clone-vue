<script setup>
import GridBoxVue from "./components/GridBox.vue";
const gridSize = [5, 6];

const createGrid = ([rows, cols]) => {
  return new Array(rows).fill(new Array(cols).fill("~"));
};

const grid = createGrid(gridSize);
console.log(grid);
</script>

<template>
  <div class="row" v-for="(row, index) in grid" :key="index">
    <span class="col" v-for="(col, index) in row" :key="index">{{ col }}</span>
  </div>
</template>

<style>
body {
  background-color: #111;
  color: white;
}
</style>
