<script setup>
import { ref, computed } from 'vue'

const player = ref('x')
const board = ref([
	['', '', ''],
	['', '', ''],
	['', '', ''],
])

const calculateWinner = (board) => {
	const lines = [
		[0, 1, 2],
		[3, 4, 5],
		[6, 7, 8],
		[0, 3, 6],
		[1, 4, 7],
		[2, 5, 8],
		[0, 4, 8],
		[2, 4, 6],
	]

	for (let i = 0; i < lines.length; i++) {
		const [a, b, c] = lines[i]

		if (board[a] && board[a] === board[b] && board[a] === board[c]) {
			return board[a]
		}
	}

	return null
}

const winner = computed(() => calculateWinner(board.value?.flat()))

const playMove = () => {
	let bestScore = -Infinity

	for (let i = 0; i < 3; i++) {
		for (let j = 0; j < 3; j++) {
			if (!board.value[i][j]) {
				console.warn(board.value[i][j])
				board.value[i][j] = 'o'
				let score = minimax(board, 0, false)
				board.value[i][j] = ''
				if (score > bestScore) {
					bestScore = score
					console.warn(`best move is ${i} and ${j}`)
				}
			}
		}
	}
}

const scores = {
	x: 10,
	o: -10,
	tie: 0,
}

const minimax = (board, depth, isMax) => {
	let result = calculateWinner(board)
	if (result !== null) {
		return scores[result]
	}

	if (isMax) {
		let bestScore = -Infinity
		for (let i = 0; i < 3; i++) {
			for (let j = 0; j < 3; j++) {
				if (!board.value[i][j]) {
					board.value[i][j] = 'o'
					let score = minimax(board, depth + 1, false)
					board.value[i][j] = ''
					bestScore = Math.max(bestScore, score)
				}
			}
		}
		console.log(bestScore)
		return bestScore
	} else {
		let bestScore = Infinity
		for (let i = 0; i < 3; i++) {
			for (let j = 0; j < 3; j++) {
				if (!board.value[i][j]) {
					board.value[i][j] = 'o'
					let score = minimax(board, depth + 1, true)
					board.value[i][j] = ''
					bestScore = Math.min(bestScore, score)
				}
			}
		}
		console.log(bestScore)
		return bestScore
	}
}

const makeMove = (x, y) => {
	if (winner.value) return

	if (board.value[x][y] !== '') return

	board.value[x][y] = player.value

	player.value = player.value === 'x' ? 'o' : 'x'

	console.log(playMove())
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
		<h1 class="mb-8 text-3xl font-bold">Tic Tac Toe</h1>
		<h2 v-if="!winner" class="text-xl mb-4">Player {{ player }}'s turn</h2>

		<div class="flex flex-col items-center mb-8 bg">
			<div v-for="(row, x) in board" :key="x" class="flex">
				<button
					v-for="(cell, y) in row"
					:key="y"
					@click="makeMove(x, y)"
					:class="`border border-gray-400 w-24 h-24 flex items-center justify-center material-icons-outlined text-4xl ${
						cell === 'x' ? 'text-pink-500' : 'text-blue-400'
					} ${
						winner
							? 'bg-gray-900 focus:outline-none hover:border-gray-400'
							: 'hover:bg-gray-600 cursor-pointer'
					}`"
					:disabled="winner"
				>
					{{ cell === 'x' ? 'close' : cell === 'o' ? 'circle' : '' }}
				</button>
			</div>
		</div>
		<div class="text-center">
			<h2 v-if="winner" class="text-6xl font-bold mb-8">
				Player {{ winner.toUpperCase() }} wins!
			</h2>
			<button
				@click="resetGame"
				class="px-4 py-2 bg-pink-500 rounded uppercase font-bold hover:bg-pink-600 duration-300"
			>
				Reset
			</button>
		</div>
	</main>
</template>

<style>
body {
	@apply bg-gray-800 text-white;
}
</style>
