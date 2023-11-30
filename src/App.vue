<script setup>
import { ref, computed } from 'vue'

const player = ref('x')
const board = ref([
	['', '', ''],
	['', '', ''],
	['', '', ''],
])

const calculateWinner = (board) => {
	const winningLines = [[0, 1, 2]]

	for (let i = 0; i < winningLines.length; i++) {
		const [a, b, c] = winningLines[i]
		if (board[a] && board[a] === board[b] && board === board[c]) {
			return board[a]
		}
		return null
	}
}

const winner = computed(() => calculateWinner(board?.value.flat()))

const makeMove = (x, y) => {
	if (winner.value) return

	if (board.value[x][y] !== '') return

	board.value[x][y] = player.value

	player.value = player.value === 'x' ? 'o' : 'x'
}

const resetGame = () => {
	board.value = [
		['', '', ''],
		['', '', ''],
		['', '', ''],
	]
	player.value = 'x'
}
</script>

<template>
	<main class="pt8 text-center">
		<h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>
		<h3 class="text-xl mb-4">Player {{ player.toUpperCase() }}'s turn</h3>

		<div class="flex flex-col items-center mb-8 bg">
			<div v-for="(row, x) in board" :key="x" class="flex">
				<div
					v-for="(cell, y) in row"
					:key="y"
					class="flex"
					@click="makeMove(x, y)"
					:class="`border border-grey w-24 h-24 hover:bg-gray-600 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${
						cell === 'x' ? 'text-pink-500' : 'text-blue-400'
					}`"
				>
					{{ cell === 'x' ? 'close' : cell === 'o' ? 'circle' : '' }}
				</div>
			</div>
		</div>
	</main>
</template>

<style>
body {
	@apply bg-gray-800 text-white;
}
</style>
