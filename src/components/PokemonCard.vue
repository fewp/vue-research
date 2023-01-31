<script lang="ts">
export default {
	name: 'PokemonCard',
	props: {
		pokemon: Object
	},
	methods: {
		reveal() {
			// fetch pokemon data from
		}
	}
};
</script>

<template>
	<div class="bg-slate-800 rounded-lg h-56 px-4 py-6 flex items-center justify-between flex-col unrevealed" @click="">
		<h3 class="text-white font-sans uppercase font-medium">
			{{ pokemon!.name }}
		</h3>
		<img 
			:src="pokemon!.sprites.front_default" 
			:alt="pokemon!.name" 
			class="object-contain h-full mt-4"
		/>
	</div>
</template>

<style scoped>
	.unrevealed img {
		opacity: 0.8;
		filter: brightness(0) invert(1) blur(10px);
	}

	h3 {
		opacity: 0.8;
	}

</style>
