<script lang="ts" setup>
const props = defineProps({
	coord: {
		type: String,
		requierd: true,
	},
	color: {
		type: String,
		requierd: true,
	},
	piece: {
		type: String,
		requierd: true,
		default: "",
	},
	selected: {
		type: Boolean,
		requierd: true,
		default: false,
	},
	move_option: {
		type: Boolean,
		requierd: true,
		default: false,
	},
	checked: {
		type: Boolean,
		requierd: true,
		default: false,
	}
});

const coord_squares = {
	'files': ['a1', 'b1', 'c1', 'd1', 'e1', 'f1', 'g1', 'h1'],
	'ranks': ['a1', 'a2', 'a3', 'a4', 'a5', 'a6', 'a7', 'a8']
}
</script>

<template>
	<div class="square black">
			<span class="piece">{{ piece }}</span>
			<span v-if="coord_squares.files.includes(coord)" class="coord file">{{ coord[0] }}</span>
			<span v-if="coord_squares.ranks.includes(coord)" class="coord rank">{{ coord[1] }}</span>
			<div v-if="move_option" class="move_option"></div>
			<div v-if="selected" class="selected"></div>
			<div v-if="checked" class="checked"></div>
	</div>
</template>

<style scoped>
.square.black {
	background-color: black;
}
</style>
