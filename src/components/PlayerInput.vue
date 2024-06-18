<template>
  <div>
    <h2>Enter Player Names</h2>
    <input v-model="playerX" placeholder="Player X" />
    <input v-model="playerO" placeholder="Player O" />
    <button @click="saveNames">Start Game</button>
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
input {
  display: block;
  margin: 10px 0;
}
</style>
