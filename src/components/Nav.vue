<script setup lang="ts">
import BootstrapVue from 'bootstrap-vue';
import 'bootstrap/dist/css/bootstrap.css';
import 'bootstrap-vue/dist/bootstrap-vue.css';
</script>

<template>
    <nav class="navbar navbar-dark bg-dark navbar-expand-lg">
        <a class="navbar-brand" href="/">
            <img src="./chess_icon.png" alt="" width="30" height="30" class="d-inline-block align-text-top">
             Chess
        </a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavDropdown">
            <ul class="navbar-nav">
                <li class="nav-item">
                    <RouterLink to="/" class="nav-link">Home</RouterLink>
                </li>
                <li class="nav-item">
                    <RouterLink to="/game/play" class="nav-link">Play</RouterLink>
                </li>
                <li class="nav-item">
                    <RouterLink to="/rules" class="nav-link">Rules</RouterLink>
                </li>
            </ul>
        </div>
    </nav>
</template>

<style scoped>
</style>