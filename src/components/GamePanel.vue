<script lang="ts" setup>
import { reactive, onMounted } from 'vue'

interface CaptureStack {
	white: string
	black: string
}

interface State {
	turn: string
	winner: string
	advantage: number
	capture_stack: CaptureStack
	undo_moves: Array<string>
	redo_moves: Array<string>
}

const state = reactive<State>({
	turn: '',
	winner: '',
	advantage: 0,
	capture_stack: null,
	undo_moves: [],
	redo_moves: [],
})

function updateGamePanel() {
	fetch('http://localhost:9000/gameInfoJson')
		.then(res => res.json())
		.then(data => {
			state.turn = data.turn
			state.winner = data.winner
			state.advantage = data.advantage
			state.capture_stack = data.capture_stack
			state.undo_moves = data.undo_moves
			state.redo_moves = data.redo_moves
		})
		.catch(err => console.log(err.message))
}

onMounted(updateGamePanel)
</script>


<template>
	<div id="info_panel" class="col-4">
		<div class="player_panel black">
			<div class="capture_stack">
				<span class="pieces"></span>
				<span class="advantage"></span>
			</div>
			<div class="name">PLAYER: Black</div>
		</div>
		<div id="move_history"></div>
		<div class="buttons">
			<button id="undo" type="button">&#8678;</button>
			<button id="redo" type="button">&#8680;</button>
			<button id="colorChangeBtn" type="button">Change Color</button>
		</div>
		<div id="winner"></div>
		<div class="player_panel white turn">
			<div class="name">PLAYER: White</div>
			<div class="capture_stack">
				<span class="pieces"></span>
				<span class="advantage"></span>
			</div>
		</div>
	</div>
</template>



<style scoped>
#info_panel {
	display: flex;
	flex-direction: column;
	justify-content: center;
	font-family: Arial, Helvetica, sans-serif;
	user-select: none;
	padding: 10px;
	margin: 0px;
	#move_history {
		max-height: 50%;
		overflow: auto;
		display: grid;
		grid-template-columns: 1fr 1fr;
		.move, .redo_move {
			padding: 1px;
		}
	}
	.buttons {
		display: flex;
		#undo, #redo, #colorChangeBtn {
			background-color: transparent;
			background-repeat: no-repeat;
			border: none;
			cursor: pointer;
			overflow: hidden;
			outline: none;
			margin-top: 0.3em;
		}
		#undo, #redo {
			flex: 1;
			font-size: 1.4em
		}
		#colorChangeBtn {
			flex: 3;
		}
	}
	.player_panel {
		padding-top: 3px;
		padding-bottom: 3px;
		&.white {
			border-top: 2px solid;
		}
		&.black {
			border-bottom: 2px solid;
		}
		.capture_stack {
			height: 1.5em;
		}
		.name {
			font-weight: bold;
			padding: 1px;
		}
	}
}


</style>
