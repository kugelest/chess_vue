<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import NavBar from './components/Nav.vue'
</script>

<template>
    <NavBar />
    <div class="container">
        <RouterView />
    </div>
</template>

<style scoped></style>
