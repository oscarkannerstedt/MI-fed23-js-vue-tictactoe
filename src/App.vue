<template>
  <div id="app">
    <PlayerInput v-if="!playersSet" @playersSet="playersSetHandler" />
    <TicTacToeBoard v-else @winner="setWinner" @gameUpdated="gameUpdatedHandler" />
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import PlayerInput from "./components/PlayerInput.vue";
import TicTacToeBoard from './components/TicTacToeBoard.vue';

const playerX = localStorage.getItem("playerX");
const playerO = localStorage.getItem("playerO");

console.log('playerX:', playerX);
console.log('playerO:', playerO);

const playersSet = ref(!!playerX && !!playerO);
const winner = ref<string | null>(null);

console.log('playersSet:', playersSet.value);

const playersSetHandler = () => {
  playersSet.value = true;
};

const setWinner = (gameWinner: string) => {
  winner.value = gameWinner;
};

const gameUpdatedHandler = () => {
  winner.value = null;
};
</script>

<style scoped>

</style>
