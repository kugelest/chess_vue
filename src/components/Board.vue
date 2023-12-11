<script lang="ts" setup>
import { reactive, onMounted } from 'vue'

interface Square {
  coord: string
  color: string
	piece: string
}

interface State {
	squares: Array<Square>
}

const state = reactive<State>({
	squares: []
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

function move() {
	fetch(`http://localhost:9000/game/play/move/${from}/${to}`)
		.then(updateBoard)
}

function squareClick(e: MouseEvent) {
	const square = e.target as HTMLElement
	const coord = square.id
	if (from === '') {
		from = coord
	}
	else {
		to = coord
		move()
		from = ''
		to = ''
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
		background: green;
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
		.selected, .move_option {
			position: absolute;
			z-index: 0;
			width: 100%;
			height: 100%;
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


