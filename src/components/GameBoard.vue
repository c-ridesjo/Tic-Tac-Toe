<script setup lang="ts">
import { ref, computed, onBeforeUnmount } from 'vue';

const player = ref('X');
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],   // 9 squares for the board
])

const score = ref<{ [key: string]: number }>({
    X: 0,
    O: 0,
});

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

const winner = computed(() => calculateWinner(board.value.flat()))

const isBoardFull = computed(() => {
  for (let i = 0; i < board.value.length; i++) {
    for (let j = 0; j < board.value[i].length; j++) {
      if (board.value[i][j] === '') {
        return false; // If any cell is empty, the board is not full
      }
    }
  }
  return true; // All cells are filled
});

const isTied = ref(false);

const makeAMove = (x: number, y: number) => {
  if (winner.value) return  // if there is a winner...

  if (board.value[x][y] !== '') return    // if the boards value is not equal to en empty box... If someone is playing you cant make a move

  board.value[x][y] = player.value

  player.value = player.value === 'X' ? 'O' : 'X'  //if this is true -> swap it to O - else its X

  const currentWinner = calculateWinner(board.value.flat());
  if (currentWinner) {
    score.value[currentWinner]++;
  } else if (isBoardFull.value) {
    isTied.value = true; 
  }
};

 const resetScore = () => {
  score.value.X = 0;
  score.value.O = 0;
}; 

const ResetGame = () => {
  board.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],   
  ]
  player.value = 'X';
  isTied.value = false;
}

const saveGameState = () => {
  const gameState = {
    player: player.value,
    board: board.value,
    score: score.value,
    isTied: isTied.value
  };

  localStorage.setItem('ticTacToeGame', JSON.stringify(gameState));
};

const loadGameState = () => {
  const savedGame = localStorage.getItem('ticTacToeGame');
  if (savedGame) {
    const gameState = JSON.parse(savedGame);
    player.value = gameState.player;
    board.value = gameState.board;
    score.value = gameState.score;
    isTied.value = gameState.isTied;
  }
};

loadGameState();  // Call this function when the game starts or refreshes

onBeforeUnmount(() => {   // Save game state whenever there is a change
  saveGameState();
});

</script>

<template>
  <main class="pt-8 text-center bg-gray-800 dark:bg-gray-900 min-h-screen dark:text-white">
    <div class="flex justify-start mx-20 mb-8">
      <div class="text-black mx-8 mt-14">
        <h1 class="mb-0 text-5xl font-bold uppercase bg-yellow-200 shadow-xl shadow-yellow-500/50">Tic</h1>
        <h1 class="mb-0 text-5xl font-bold uppercase mt-2 bg-yellow-200 shadow-xl shadow-yellow-500/50">Tac</h1>
        <h1 class="mb-0 text-5xl font-bold uppercase mt-2 bg-yellow-200 shadow-xl shadow-yellow-500/50">Toe</h1>
      </div>

      <div class="flex justify-around mx-20">
      <div>
        <h3 class="text-1xl font-bold text-white">Player {{ player }}'s turn</h3>

        <div class="flex flex-col items-center mb-6">
          <div v-for="(row, x) in board" :key="x" class="flex">

            <div v-for="(cell, y) in row" :key="y" @click="makeAMove(x, y)"
              :class="`border border-white w-20 h-20 hover:bg-grey-700 flex items-center
                              justify-center material-icons-outlined text-4x1 cursor-pointer ${cell === 'X' ? 'text-green-500' : 'text-pink-400'}`">
              {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
            </div>
          </div>
        </div>

        <h2 v-if="winner" class="border-2 border-amber-500/20 text-6x1 font-bold mb-5 shadow-lg shadow-amber-500/50 text-amber-200"> Player {{ winner }} wins!</h2>

        <h2 v-if="isTied" class="text-2xl font-bold mb-5 text-amber-200">Tied game!</h2>

        <button @click="ResetGame" class="px-4 mt-3 py-2 bg-green-500 rounded uppercase 
        font-bold hover:bg-green-600 duration-300 ">Start game</button>
      </div>

      <div class="mb-8 mx-20">
        <h2 class="text-1xl font-bold text-white mt-14 mx-12">Score</h2>
          <div class="text-white">
            <p>Player X: {{ score.X }}</p>
            <p>Player O: {{ score.O }}</p>
            <button @click="resetScore" class="px-2 py-1 bg-pink-400 rounded uppercase 
              font-bold hover:bg-pink-500 duration-300 mt-4 text-black">Reset</button>
          </div>
        </div>
      </div>
    </div>

  </main>
</template>
