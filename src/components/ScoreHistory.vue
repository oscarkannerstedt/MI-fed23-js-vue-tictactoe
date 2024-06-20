<template>
    <div>
      <h2>Score History</h2>
      <ul>
        <li v-for="(score, player) in scores" :key="player">
          {{ getPlayerName(player) }}: {{ score }}
        </li>
      </ul>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: "ScoreHistory",
  props: {
    scores: {
      type: Object,
      required: true,
    },
  },
  methods: {
    getPlayerName(player: string): string {
      if (player === "X") {
        return localStorage.getItem("playerX") || "Player X";
      } else if (player === "O") {
        return localStorage.getItem("playerO") || "Player O";
      } else {
        return "Draws";
      }
    },
  },
});
</script>