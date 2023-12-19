<script setup lang="ts">
</script>

<template>
	<div class="container">
		<div class="div1"></div>
		<div class="div2"></div>
	</div>
</template>

<style scoped>
.container {
	display: flex;
	height: calc(100vh - 50px);
	/* height: 100vh; */
}
.div1 {
	flex: 1;
	height: 100%;
	/* height: 100vh; */
	/* width: 500px; */
}
.div2 {
	flex: 1;
	height: 100%;
	background-color: blue;
}
</style>
