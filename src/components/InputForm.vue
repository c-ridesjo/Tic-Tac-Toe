<script setup lang="ts">
import { ref } from 'vue';

const playerXName = ref('');
const playerOName = ref('');
const gameStarted = ref(false);

const startGame = () => {
  if (playerXName.value && playerOName.value) {
    gameStarted.value = true;
  }
};
</script>

<template>
  <div class="container flex flex-col items-center pt-16">
    <form class="mb-8">
      <label for="playerXName" class="mb-2 font-bold">Player X name:</label>
      <input type="text" id="playerXName" v-model="playerXName" required class="px-2 py-1 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
      <label for="playerOName" class="mt-4 mb-2 font-bold">Player O name:</label>
      <input type="text" id="playerOName" v-model="playerOName" required class="px-2 py-1 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
      <button @click="startGame" class="px-4 py-2 mt-4 bg-green-500 text-white font-bold rounded-md hover:bg-green-600 focus:outline-none focus:ring-2 focus:ring-green-500">
        Start game
      </button>
    </form>

    <main class="game-board bg-gray-800 text-white p-8 rounded-md" v-if="gameStarted">
      <!-- Game code here -->
    </main>
  </div>
  </template> 
  
  <style scoped></style>
  