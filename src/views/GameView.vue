<script setup lang="ts">
import Board from '../components/Board.vue'
import GamePanel from '../components/GamePanel.vue'
</script>

<template>
	<main>
		<!-- <div class="cont"> -->
			<Board class="board" />
			<GamePanel class="panel" />
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
.panel {
	background-color: green !important;
}

</style>
