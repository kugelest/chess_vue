<script lang="ts" setup>
import { reactive, onMounted } from 'vue'
import Square from './Square.vue';

interface Square {
  coord: string;
  color: string;
  piece: string;
  selected: boolean;
  move_option: boolean;
  checked: boolean;
}

interface State {
	squares: Array<Square>
	move_options: Array<string>
	selected: string
	checked_coord: string
}

const state = reactive<State>({
	squares: [],
	move_options: [],
	selected: '',
	checked_coord: ''
})

let from = ''
let to = ''


function updateBoard() {
	fetch('http://localhost:9000/squaresJson')
		.then(res => res.json())
		.then(data => state.squares = data)
		.catch(err => console.log(err.message))
}

function checkedCoord() {
	fetch(`http://localhost:9000/checkedCoordJson`)
		.then(res => res.json())
		.then(data => state.checked_coord = data)
		.catch(err => console.log(err.message))
}

function moveOptions() {
	fetch(`http://localhost:9000/moveOptionsJson/${from}`)
		.then(res => res.json())
		.then(data => state.move_options = data)
		.catch(err => console.log(err.message))
}

function move() {
	fetch(`http://localhost:9000/game/play/move/${from}/${to}`)
		.then(updateBoard)
		.then(checkedCoord)
		.then(clearMove)
}

function clearMove() {
	from = ''
	to = ''
	state.selected = ''
	state.move_options = []
}

function handleSquareClick(coord: string) {
	if (from == '') {
		from = coord
		state.selected = coord
		moveOptions()
	}
	else {
		to = coord
		move()
	}
}

onMounted(updateBoard)
</script>



<template>
	<div id="board">
		<Square v-for="square in state.squares"
			:coord="square.coord"
			:color="square.color"
			:piece="square.piece"
			:selected="state.selected === square.coord"
			:move_option="state.move_options.includes(square.coord)"
			:checked="state.checked_coord === square.coord"
			@squareClick="handleSquareClick" />
	</div>
</template>



<style scoped>
#board {
	font-family: Arial;
	display: grid;
  grid-template-columns: repeat(8, 1fr);
	grid-auto-rows: 1fr;
}
</style>


