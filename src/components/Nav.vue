<script setup lang="ts">
</script>

<template>
	<header>
		<img src="@/assets/images/chess_icon.png" />
		<div class="title"> Chess </div>
		<nav>
			<RouterLink to="/" class="nav-link">Home</RouterLink>
			<RouterLink to="/game/play" class="nav-link">Play</RouterLink>
			<RouterLink to="/rules" class="nav-link">Rules</RouterLink>
			<RouterLink to="/test" class="nav-link">Test</RouterLink>
		</nav>
	</header>
</template>

<style scoped>
header {
	display: flex;
	align-items: center;
	container-type: size;
	height: 50px;
	background-color: rgb(40, 40, 40);
}

img {
	height: 100cqh;
}

.title {
	color: white;
	font-size: 60cqh;
	margin-left: 5px;
	margin-right: 30px;
}

nav {
	display: flex;
	gap: 25px;
}

.nav-link {
	color: grey;
}

.router-link-active {
	color: white !important;
}

</style>
