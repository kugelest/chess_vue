<script setup lang="ts">
import Board from '../components/Board.vue'
import GamePanel from '../components/GamePanel.vue'
import { reactive, onMounted } from 'vue'

interface Square {
  coord: string;
  color: string;
  piece: string;
  selected: boolean;
  move_option: boolean;
  checked: boolean;
}

interface CaptureStack {
	white: string
	black: string
}

interface State {
	squares: Array<Square>,
	turn: string,
	winner: string,
	capture_stack: CaptureStack,
	advantage: number,
	undo_moves: Array<String>,
	redo_moves: Array<String>,
}

const state = reactive<State>({
	squares: [],
	turn: '',
	winner: '',
	capture_stack: null,
	advantage: 0,
	undo_moves: [],
	redo_moves: []
})

function connectWebSocket() {
	var websocket = new WebSocket("ws://localhost:9000/websocket");
	// websocket.setTimeout

	websocket.onopen = function(e) {
		console.log("Connection established!");
	};

	websocket.onclose = function(e) {
		console.log("Connection closed!");
	};

	websocket.onerror = function(e) {
		console.log("Error: " + e.data);
	};

	websocket.onmessage = function(e) {
		if (typeof e.data === "string") {
			const response = JSON.parse(e.data)
			if (Array.isArray(response)) {
				state.squares = response
			}
			else {
				console.log(response);
				state.turn = response.turn
				state.winner = response.winner
				state.capture_stack = response.capture_stack
				state.advantage = response.advantage
				state.undo_moves = response.undo_moves
				state.redo_moves = response.redo_moves
			}
    }
		else {
			console.log(e.data);
		}
	};
}

function initBoard() {
	fetch('http://localhost:9000/squaresJson')
		.then(res => res.json())
		.then(data => state.squares = data)
		.catch(err => console.log(err.message))
}

onMounted(() => {
	initBoard()
	connectWebSocket()
})

</script>

<template>
	<main>
		<!-- <div class="cont"> -->
			<Board class="board" :squares=state.squares />
			<GamePanel class="panel" :turn=state.turn :winner=state.winner :advantage=state.advantage :capture_stack=state.capture_stack :undo_moves=state.undo_moves :redo_moves=state.redo_moves />
		<!-- </div> -->
	</main>
</template>

<style scoped>
main {
	display: flex;
	justify-content: center;
	flex-wrap: wrap;
	/* width: 100%; */
	/* height: calc(100vh - 50px); */
  /* grid-template-columns: 1fr 1fr; */
  /* grid-template-rows: 1fr auto; */
}

.board {
	/* height: min(calc(100vh - 50px), calc(100vw / 2)); */
	/* width: min(calc(100vh - 50px), calc(100vw / 2)); */
	height: min(calc(100vh - 50px), 100vw);
	width: min(calc(100vh - 50px), 100vw);
}

</style>
