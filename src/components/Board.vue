<script lang="ts" setup>
import { reactive, onMounted } from 'vue'

interface Square {
  coord: string
  color: string
	piece: string
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

const coord_squares = {
	'files': ['a1', 'b1', 'c1', 'd1', 'e1', 'f1', 'g1', 'h1'],
	'ranks': ['a1', 'a2', 'a3', 'a4', 'a5', 'a6', 'a7', 'a8']
}

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

function squareClick(e: MouseEvent) {
	const square = e.target as HTMLElement
	const coord = square.id
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
		<div v-for="square in state.squares" :id="square.coord" :class="square.color" class="square" @click="squareClick">
			<span class="piece">{{ square.piece }}</span>
			<span v-if="coord_squares.files.includes(square.coord)" class="coord file">{{ square.coord[0] }}</span>
			<span v-if="coord_squares.ranks.includes(square.coord)" class="coord rank">{{ square.coord[1] }}</span>
			<div v-if="state.move_options.includes(square.coord)" class="move_option"></div>
			<div v-if="state.selected == square.coord" class="selected"></div>
			<div v-if="state.checked_coord == square.coord" class="checked"></div>
		</div>
	</div>
</template>



<style scoped>
#board {
	font-family: Arial;
	display: grid;
  	grid-template-columns: repeat(8, 1fr);
	grid-auto-rows: 1fr;
	.square {
		display: flex;
		align-items: center;
		justify-content: center;
		container-type: size;
		&.white {
			background-color: white;
			.coord {
				color: darkgrey;
			}
		}
		&.black {
			background-color: darkgrey;
			.coord {
				color: white;
			}
		}
		.selected, .move_option, .checked {
			position: absolute;
			z-index: 0;
			width: 100%;
			height: 100%;
			pointer-events: none;
		}
		.selected {
			background-color: rgba(66, 126, 245, 0.7);
		}
		.move_option {
			background-color: rgba(72, 245, 66, 0.7);
		}
		.checked {
			background-color: rgba(212, 42, 42, 0.7);
		}
		.coord {
			z-index: 1;
			font-size: 13cqh;
			font-weight: bold;
			&.rank {
				align-self: start;
				margin-left: 4cqh;
				margin-right: auto;

			}
			&.file {
				align-self: end;
				margin-right: 4cqh;
				margin-left: auto;
			}
		}
		.piece {
			position: absolute;
			z-index: 1;
			font-size: 100cqh;
			user-select: none;
			pointer-events: none;
		}
	}
	&::before {
    content: '';
    width: 0;
    padding-bottom: 100%;
    grid-row: 1 / 1;
    grid-column: 1 / 1;
	}
	*:first-child {
    grid-row: 1 / 1;
    grid-column: 1 / 1;
	}
}
</style>


