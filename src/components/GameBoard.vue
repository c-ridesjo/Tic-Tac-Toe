<script setup lang="ts">
  import { ref, computed } from 'vue';

  const player = ref('X');
  const board = ref ([
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],   // 9 squares for the board
  ])

  const calculateWinner = (squares: string[]): string | null => {
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

const winner = computed (() => calculateWinner (board.value.flat()))

const makeAMove = (x:number, y:number) => {
  if (winner.value) return  // if we have a winner...

  if (board.value[x][y] !== '') return    // if the boards value is not equal to en empty box... If someone is playing you cant make a move

  board.value[x][y] = player.value

  player.value = player.value === 'X' ? 'O' : 'X'  //if this is true -> swap it to O - else its X
}
</script>

<template>
  <h1>Testing!</h1>

</template>

<style scoped>

</style>