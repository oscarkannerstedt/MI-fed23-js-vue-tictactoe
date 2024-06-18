<template>
    <div>
        <div class="board">
            <div v-for="(cell, index) in board" :key="index" class="cell" @click="makeMove(index)">
                {{ cell }}
            </div>
        </div>
        <p v-if="winner">{{ winnerMessage }}</p>
        <p v-if="isDraw">It's a draw!</p>
        <button v-if="winner || isDraw" @click="startNewGame">Start New Game</button>
        <button @click="goBack">Go Back</button>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch, computed } from 'vue';

export default defineComponent({
    name: "TicTacToeBoard",
    emits: ["winner", "gameUpdated", "goBack"],
    setup(_, { emit }) {
        const board = ref<string[]>(JSON.parse(localStorage.getItem("board") || "[]").length ? JSON.parse(localStorage.getItem("board") || "[]") : Array(9).fill(""));
        const currentPlayer = ref<"X" | "O">(localStorage.getItem("currentPlayer") as "X" | "O" || "X");
        const winner = ref<string | null>(null);
        const isDraw = ref(false);

        const checkWinner = () => {
            const winningsCombination = [
                [0, 1, 2],
                [3, 4, 5],
                [6, 7, 8],
                [0, 3, 6],
                [1, 4, 7],
                [2, 5, 8],
                [0, 4, 8],
                [2, 4, 6],
            ];

            for (const combo of winningsCombination) {
                const [a, b, c] = combo;
                if(board.value[a] && board.value[a] === board.value[b] && board.value[a] === board.value[c]) {
                    return board.value[a];
                }
            }

            return board.value.every(cell => cell) ? "Draw" : null;
        };

        const makeMove = (index: number) => {
            if (board.value[index] === "" && !winner.value && !isDraw.value) {
                board.value[index] = currentPlayer.value;
                const result = checkWinner();
                if (result) {
                    if (result === "Draw") {
                        isDraw.value = true;
                    } else {
                        winner.value = result;
                        updateScores(result);
                    }
                    emit("winner", result);
                } else {
                    currentPlayer.value = currentPlayer.value === "X" ? "O" : "X";
                }
                localStorage.setItem("board", JSON.stringify(board.value));
                localStorage.setItem("currentPlayer", currentPlayer.value);
            }
        };

        const startNewGame = () => {
            board.value = Array(9).fill("");
            currentPlayer.value = "X";
            winner.value = null;
            isDraw.value = false;
            localStorage.removeItem("board");
            localStorage.removeItem("currentPlayer");
            emit("gameUpdated");
        };

        const updateScores = (result: string) => {
            const scores = JSON.parse(localStorage.getItem("scores") || "{}");
            if (result === "X" || result === "O") {
                scores[result] = (scores[result] || 0) +1;
            } else if (result === "Draw") {
                scores["Draws"] = (scores["Draws"] || 0) +1;
            }
            localStorage.setItem("scores", JSON.stringify(scores));
        };

        const clearPlayerData = () => {
            localStorage.removeItem("playerX");
            localStorage.removeItem("playerO");
            localStorage.removeItem("board");
            localStorage.removeItem("currentPlayer");
        };

        const goBack = () => {
            startNewGame();
            clearPlayerData();
            emit("goBack");
        };

        watch(winner, (newWinner) => {
            if (newWinner) {
                emit("winner", newWinner);
            }
        });

        const winnerMessage = computed(() => {
            if (winner.value) {
                const playerName = winner.value === "X" ? localStorage.getItem("playerX") :  localStorage.getItem("playerO");
                return `${playerName} wins!`;
            }
            return "";
        });

        return {
            board,
            currentPlayer,
            winner,
            isDraw,
            makeMove,
            startNewGame,
            goBack,
            winnerMessage,
        };
    },
});
</script>

<style scoped>
.board {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    gap: 5px;
}

.cell {
    width: 100px;
    height: 100px;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #f0f0f0;
    cursor: pointer;
    font-size: 24px;
}
</style>