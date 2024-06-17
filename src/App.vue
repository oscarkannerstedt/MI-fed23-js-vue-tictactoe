<template>
  <div id="app">
    <PlayerInput v-if="!playersSet" @playersSet="playersSetHandler" />
  </div>
</template>

<script setup lang="ts">
import { defineComponent, ref } from "vue";
import PlayerInput from "./components/PlayerInput.vue";
</script>

<style scoped>

</style>
