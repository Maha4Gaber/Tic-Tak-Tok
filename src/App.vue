<script setup>
import { ref, computed } from 'vue'
const player = ref('X')
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', '']
])
const CalculateWinner = (board) => {
  const lines = [[0, 1, 2],[3, 4, 5],[6, 7, 8],[0, 3, 6],[1, 4, 7],[2, 5, 8],[0, 4, 8],[2, 4, 6]]
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i]
    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a]
    }
  }
  return null
}
const winner = computed(() => CalculateWinner(board.value.flat()))
const MakeMove = (x, y) => {
	if (winner.value) return
	if (board.value[x][y]) return
	board.value[x][y] = player.value
	player.value = player.value === 'X' ? 'O' : 'X'
}
const ResetGame = () => {
	board.value = [
		['', '', ''],
		['', '', ''],
		['', '', '']
	]
	player.value = 'X'
}
</script>

<template>
	<main class="pt-8 text-center">
		<h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>

		<h3 class="text-xl mb-4">Player {{ player }}'s turn</h3>

		<div class="first">
			<div 
				v-for="(row, x) in board" 
				:key="x"
				class="row">
				<div 
					v-for="(cell, y) in row" 
					:key="y" 
					@click="MakeMove(x, y)" 
          :style="` ${cell === 'X' ? 'color:red' : 'color:lightblue'}`"
					:class="`child` ">
          
					{{ cell === 'X' ? 'X' : cell === 'O' ? 'O' : '' }}
				</div>
			</div>
		</div>

		<div class="text-center">
			<h2 v-if="winner" class="text-6xl font-bold mb-8">Player '{{ winner }}' wins!</h2>
			<button @click="ResetGame" class="px-4 py-2 bg-pink-500 rounded uppercase font-bold hover:bg-pink-600 duration-300">Reset</button>
		</div>
	</main>
</template>

<style>
body {
	@apply bg-gray-800 text-white;
}

main{
  padding-top: 8rem;
  text-align: center;
  background-color: rgb(19, 19, 27);
  color: aliceblue;
  height: 100vh;
  
}
main h1
{
  margin-bottom: 8rem;
  font-size: 3rem;
  font-weight: bold;
  text-transform: uppercase ;
}
main h3 
{
  font-size: 2rem;
  margin-bottom: 4rem;
}
.first
{
  display: flex;
  margin-bottom: 8rem;
  justify-content: center;
  /* flex-wrap: wrap; */
  width: 50%;
  margin: 0 auto 20px;
}
.first .row
{
  display: flex;
  flex-wrap: wrap;
  border: 2px solid rgb(19, 19, 27);
}
.child{
  display: flex;
  background-color: rgb(19, 19, 27);
  flex-basis: 1;
  height: 42px;
  cursor: pointer;
  border: 2px solid whitesmoke;
  text-align: center;
  align-items: center;
  justify-content: center;
}
</style>