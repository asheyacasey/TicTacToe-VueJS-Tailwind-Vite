<script setup>
//logic 
import { ref, computed } from 'vue' 

const player = ref('X');
const board = ref ([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],
])

const tttWinner = (squares) => {
  ///lines from 12 to 31 is from https://reactjs.org/tutorial/tutorial.html, a given source code declare a winner.
  const lines = [
    //these lines determine how you win like if you have a line of 0, 1, 2
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}

const winner = computed(() => tttWinner(board.value.flat()))//flat will turn all those array into singular array.

//select our move and we're going to pass the x and y position of where we click
const MakeMove = (x, y) => {
  if (winner.value) return
 
  if (board.value[x][y] != '') return

  board.value[x][y] = player.value

  player.value = player.value === 'X' ? 'O' : 'X' //ternary operator
}


const ResetGame = () => {
  board.value = [
      ['', '', ''],
      ['', '', ''],
      ['', '', ''],
  ]
  player.value = 'X'
}
</script>

<template>
 <main class="pt-8 text-center bg-gray-800 min-h-screen text-white">
  <h1 class="mb-8 text-4xl font-bold uppercase">Tic Tac Toe</h1>
  <h3 class="text-xl mb-4">Player {{ player }}'s Turn</h3>

  <div class="flex flex-col items-center mb-8">
    <div  
        v-for="(row, x) in board"
        :key="x" 
        class="flex">
        
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="MakeMove(x,y)"
          :class="`border border-white w-20 h-20 hover:bg-gray-700 flex
          items-center justify-center material-icons-outlined text-4xl
          cursor-pointer ${ cell === 'X' ? 'text-pink-500' : 'text-blue-500'}` "
        >
        {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
      </div>
    </div>
  </div>

  <div class="text-center">
			<h2 v-if="winner" class="text-6xl font-bold mb-8">Player '{{ winner }}' wins!</h2>
			<button @click="ResetGame" class="px-4 py-2 bg-pink-500 rounded uppercase font-bold hover:bg-pink-600 duration-300">Reset</button>
		</div>


 </main>
</template>


