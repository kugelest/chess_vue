<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/game">Game</RouterLink>
      </nav>
  </header>

  <RouterView />
</template>

<style scoped>

nav {
	margin: 10px;
	a {
		padding: 10px;
		color: green;
		&.router-link-exact-active {
			background: grey;
		}
	}
}
</style>
