<template>
  <div class="player-input-container">
    <h2>Enter Player Names</h2>
    <div class="input-wrapper">
      <input v-model="playerX" placeholder="Player X" class="player-input" />
    </div>
    <div class="input-wrapper">
    <input v-model="playerO" placeholder="Player O" class="player-input" />
  </div>
  <div class="button-wrapper">
    <button @click="saveNames">Start Game</button>
  </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: "PlayerInput",
  emits: ["playersSet"],
  setup(_, { emit }) {
    const playerX = ref(localStorage.getItem("playerX") || "");
    const playerO = ref(localStorage.getItem("playerO") || "");

    const saveNames = () => {
      localStorage.setItem("playerX", playerX.value);
      localStorage.setItem("playerO", playerO.value);
      emit("playersSet");
    };

    return {
      playerX,
      playerO,
      saveNames,
    };
  },
});
</script>

<style scoped>
.player-input-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.input-wrapper {
  margin: 3px;
}

.player-input {
  padding: 3px;
  text-align: center;
}

.button-wrapper {
  margin: 7px;
}

</style>
