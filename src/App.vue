<script setup>
import GridBox from "./components/GridBox.vue";
import KeyboardKey from "./components/KeyboardKey.vue";
import { ref } from "vue";
import { wordList } from "./assets/words5";
const cLevel = require("./utils/cLevel");

const createGrid = ([rows, cols]) => {
  const grid = new Array(rows);
  for (let row = 0; row < grid.length; row++) {
    grid[row] = new Array(cols);
    for (let col = 0; col < grid[row].length; col++) {
      grid[row][col] = {
        char: " ",
        correctness: cLevel.unchecked,
      };
    }
  }
  return grid;
};

const inputCharToGrid = (char) => {
  if (currentCol < wordLength) {
    grid.value[currentRow][currentCol].char = char;
    currentCol++;
  }
};

const deleteLastCharFromGrid = () => {
  if (currentCol > 0) {
    currentCol--;
    grid.value[currentRow][currentCol].char = " ";
  }
};

const submitWord = (word) => {
  if (
    currentCol === grid.value[currentRow].length &&
    wordList.indexOf(joinRow(grid.value[currentRow])) > -1
  ) {
    const remainingGuess = [];
    const remainingWord = [];
    for (let i = 0; i < word.length; i++) {
      const winningChar = winningWord.charAt(i);
      const guessChar = word[i].char;
      if (winningChar === guessChar) {
        grid.value[currentRow][i].correctness = cLevel.correct;
        guessedLetters.push([grid.value[currentRow][i].char, cLevel.correct]);
      } else {
        remainingWord.push(winningChar);
        remainingGuess.push([guessChar, i]);
      }
    }
    for (let j = 0; j < remainingGuess.length; j++) {
      if (remainingWord.indexOf(remainingGuess[j][0]) > -1) {
        grid.value[currentRow][remainingGuess[j][1]].correctness =
          cLevel.yellow;
        guessedLetters.push([grid.value[currentRow][j].char, cLevel.yellow]);
      } else {
        guessedLetters.push([grid.value[currentRow][j].char, cLevel.incorrect]);
        grid.value[currentRow][remainingGuess[j][1]].correctness =
          cLevel.incorrect;
      }
    }
    console.log(guessedLetters);
    keyboard.value = updateKeyboardBackgrounds();
    currentRow++;
    currentCol = 0;
  } else {
    playErrorAnimation();
  }
};

const updateKeyboardBackgrounds = () => {
  let keyboard = [
    ["q", "w", "e", "r", "t", "y", "u", "i", "o", "p"],
    ["a", "s", "d", "f", "g", "h", "j", "k", "l"],
    ["enter", "z", "x", "c", "v", "b", "n", "m", "<"],
  ];

  const letters = guessedLetters.map(([letter, correctness]) => letter);
  const correctnesses = guessedLetters.map(
    ([letter, correctness]) => correctness
  );

  for (let row = 0; row < keyboard.length; row++) {
    for (let col = 0; col < keyboard[row].length; col++) {
      const letterPos = letters.indexOf(keyboard[row][col]);
      if (letterPos === -1) {
        keyboard[row][col] = {
          key: keyboard[row][col],
          correctness: cLevel.unchecked,
        };
      } else {
        keyboard[row][col] = {
          key: keyboard[row][col],
          correctness: correctnesses[letterPos],
        };
      }
    }
  }
  return keyboard;
};

const joinRow = (row) => {
  let word = "";
  for (let i = 0; i < row.length; i++) {
    word += row[i].char;
  }
  return word;
};

const playErrorAnimation = () => {
  for (let i = 0; i < grid.value[currentRow].length; i++) {
    grid.value[currentRow][i].correctness = cLevel.error;
  }
  setTimeout(() => {
    for (let i = 0; i < grid.value[currentRow].length; i++) {
      grid.value[currentRow][i].correctness = cLevel.unchecked;
    }
    forceRerender();
  }, 300);
};

const handleKeyboard = (event) => {
  console.log(event.key);
  if (event.key === "Enter") {
    submitWord(grid.value[currentRow]);
  }
  if (event.keyCode >= 57 && event.keyCode <= 90) {
    inputCharToGrid(event.key);
  }
  if (event.keyCode === 8) {
    event.preventDefault();
    deleteLastCharFromGrid();
  }
  forceRerender();
};

const handleClicks = (event) => {
  if (event.target.classList.contains("keyboard-key")) {
    const letter = event.target.innerHTML.toLowerCase();
    if (letter === "enter") {
      submitWord(grid.value[currentRow]);
    } else if (letter === "&lt;") {
      deleteLastCharFromGrid();
    } else {
      inputCharToGrid(letter);
    }
  }
};

const componentKey = ref(0);

const forceRerender = () => {
  componentKey.value += 1;
};

const wordLength = 5;
const numberOfGuesses = 6;
const gridSize = [numberOfGuesses, wordLength];
const grid = ref(createGrid(gridSize));
const guessedLetters = [];
let keyboard = ref(updateKeyboardBackgrounds());
const winningWord = "kappa";
let currentRow = 0;
let currentCol = 0;
document.body.addEventListener("keydown", handleKeyboard);
document.body.addEventListener("click", handleClicks);
</script>

<template>
  <div class="container" :key="componentKey">
    <div class="grid">
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
            :data="col"
          />
        </div>
      </div>
    </div>
    <div class="keyboard">
      <div>
        <div
          class="keyboard-row"
          v-for="(kRow, index) in keyboard"
          :key="'kRow' + index"
        >
          <KeyboardKey
            v-for="(kCol, index) in kRow"
            :data="kCol"
            :key="'kCol' + index"
          ></KeyboardKey>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
body {
  background-color: #222;
  color: white;
  font-size: 30px;
  font-family: "Clear Sans", "Helvetica Neue", Arial, sans-serif;
  font-weight: bold;
}

.grid {
  display: flex;
  justify-content: center;
  align-items: center;
}

.row {
  display: flex;
}

.keyboard {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 8px;
}

.keyboard-row {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
