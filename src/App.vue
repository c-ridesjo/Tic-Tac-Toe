<script setup lang="ts">
  import { ref, computed } from 'vue';

  const player = ref('X');
  const board = ref ([
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],   // 9 squares for the board
  ])

  const calculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]   // lines for winning
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];  // the lines redefines to a, b and c
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}

</script>

<template>
  <h1>Testing!</h1>

</template>

<style scoped>

</style>
