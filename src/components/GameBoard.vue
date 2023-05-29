<script setup lang="ts">
  import { ref, computed } from 'vue';

  const player = ref('X');
  const board = ref ([
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],   // 9 squares for the board
  ])

  const score = ref({
    X: 0,
    O: 0,
  })

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

  const currentWinner = calculateWinner(board.value.flat());
  if (currentWinner) {
    score.value[currentWinner]++;
  }
};

const ResetGame = () => {
  board.value= [
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],   // 9 squares for the board
  ]
  player.value = 'X'
}
</script>

<template>
  <main class="pt-8 text-center bg-gray-800 dark:bg-gray-900 min-h-screen dark:text-white">


    <h1 class="mb-8 text-5x1 font-bold uppercase text-white">Tic Tac Toe</h1>
    <h3 class="text-x1 mb-4 text-white">Player {{ player }}'s turn</h3>

    <div class="flex flex-col items-center mb-8">
      <div
        v-for="(row, x) in board"
        :key="x"
        class="flex">

        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="makeAMove(x, y)"
          :class="`border border-white w-20 h-20 hover:bg-grey-700 flex items-center
          justify-center material-icons-outlined text-4x1 cursor-pointer ${ cell === 'X' ? 'text-green-500' : 'text-pink-400' }`">
          {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
        </div>
      </div>
      <div class="mb-8">
        <h2 class="text-2xl font-bold text-white">Score</h2>
        <div class="text-white">
          <p>Player X: {{ score.X }}</p>
          <p>Player O: {{ score.O }}</p>
        </div>
  </div>
    </div>
    
    <h2 v-if="winner" class="text-6x1 font-bold mb-8 text-white">Player {{ winner }} wins!</h2>

    <button @click="ResetGame" class="px-4 py-2 bg-green-500 rounded uppercase 
    font-bold hover:bg-green-600 duration-300 ">Restart game</button>
  </main>

</template>
