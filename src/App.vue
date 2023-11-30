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
	<div>Henlo!</div>
</template>

<style scoped></style>
