<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import NavBar from './components/Nav.vue'
</script>

<template>
	<NavBar />
	<RouterView />
</template>

<style>
body {
	margin: 0;
	padding: 0;
	font-family: Arial, Helvetica, sans-serif;
}
html {
	margin: 0;
	padding: 0;
}
</style>
