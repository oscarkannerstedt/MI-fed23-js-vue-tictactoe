<template>
  <div>
    <h2>Enter Player Names</h2>
    <input v-model="playerX" placeholder="Player X" />
    <input v-model="playerO" placeholder="Player O" />
    <button @click="saveNames">Start Game</button>
    <button @click="toggleScores">{{ showScores ? 'Hide Scores' : 'Show Scores' }}</button>
    <button v-if="showScores" @click="resetScores">Reset Scores</button>
    <ScoreHistory v-if="showScores" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import ScoreHistory from './ScoreHistory.vue';

export default defineComponent({
  name: "PlayerInput",
  components: {
    ScoreHistory,
  },
  emits: ["playersSet"],
  setup(_, { emit }) {
    const playerX = ref(localStorage.getItem("playerX") || "");
    const playerO = ref(localStorage.getItem("playerO") || "");
    const showScores = ref(false);

    const saveNames = () => {
      localStorage.setItem("playerX", playerX.value);
      localStorage.setItem("playerO", playerO.value);
      emit("playersSet");
    };

    const resetScores = () => {
      localStorage.removeItem("scores");
      showScores.value = false;
    };

    const toggleScores = () => {
      showScores.value = !showScores.value;
    };

    return {
      playerX,
      playerO,
      showScores,
      saveNames,
      resetScores,
      toggleScores,
    };
  },
});
</script>

<style scoped>
input {
  display: block;
  margin: 10px 0;
}
</style>
