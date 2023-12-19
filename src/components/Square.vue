<script lang="ts" setup>

interface Square {
  coord: string;
  color: string;
  piece: string;
  selected: boolean;
  move_option: boolean;
  checked: boolean;
}

const props = defineProps<{
  coord: string;
  color: string;
  piece: string;
  selected: boolean;
  move_option: boolean;
  checked: boolean;
}>()

const coord_squares = {
	'files': ['a1', 'b1', 'c1', 'd1', 'e1', 'f1', 'g1', 'h1'],
	'ranks': ['a1', 'a2', 'a3', 'a4', 'a5', 'a6', 'a7', 'a8']
}

interface Colors {
	white: string;
	black: string;
	other_color: string; // Make it optional since it's added dynamically
}

const colors: Colors = {
	white: 'white',
	black: 'darkgrey',
	other_color: ''
}
colors.other_color = (props.color === 'white') ? colors.black : colors.white


const emit = defineEmits<{
  squareClick: [square: string]
}>()

function handleClick() {
	emit('squareClick', props.coord)
}

</script>



<template>
	<div class="square" @click="handleClick">
		<span class="piece">{{ piece }}</span>
		<span v-if="coord_squares.files.includes(coord)" class="coord file">{{ coord[0] }}</span>
		<span v-if="coord_squares.ranks.includes(coord)" class="coord rank">{{ coord[1] }}</span>
		<div v-if="move_option" class="move_option"></div>
		<div v-if="selected" class="selected"></div>
		<div v-if="checked" class="checked"></div>
	</div>
</template>



<style scoped>
.square {
	background-color: v-bind(colors[color]);
	display: flex;
	align-items: center;
	justify-content: center;
	container-type: size;

	.piece {
		position: absolute;
		z-index: 1;
		font-size: 100cqh;
		user-select: none;
		pointer-events: none;
	}

	.coord {
		z-index: 1;
		font-size: 13cqh;
		font-weight: bold;
		color: v-bind(colors.other_color);

		&.rank {
			align-self: start;
			margin-left: 4cqh;
			margin-right: auto;

		}

		&.file {
			align-self: end;
			margin-right: 4cqh;
			margin-left: auto;
		}
	}

	.selected,
	.move_option,
	.checked {
		position: absolute;
		z-index: 0;
		width: 100%;
		height: 100%;
		pointer-events: none;
	}

	.selected {
		background-color: rgba(66, 126, 245, 0.7);
	}

	.move_option {
		background-color: rgba(72, 245, 66, 0.7);
	}

	.checked {
		background-color: rgba(212, 42, 42, 0.7);
	}
}
</style>
