<script lang="ts">
export default {
	name: 'PokemonCard',
	props: {
		pokemon: Object,
		revealed: Boolean
	},
	methods: {
		reveal(event: any) {
			const card = event.target.closest('.pokemon-card');			

			card.classList.contains('unrevealed') ?
				card.classList.remove('unrevealed') :
				card.classList.add('unrevealed');
		}
	}
};
</script>

<template>
	<div class="bg-slate-100 rounded-lg h-56 px-4 py-6 flex items-center justify-between flex-col pokemon-card relative" 
		:id="pokemon!.name"
		:class="[!revealed ? 'unrevealed' : '', pokemon!.types.map((type: any) => type.type.name).join(' ')]" 
		@click="reveal($event)">
		<h3 class="text-slate-900 font-sans uppercase font-bold select-none pointer-events-none">
			{{ pokemon!.name }}
		</h3>
		<img 
			:src="pokemon!.sprites.front_default" 
			:alt="pokemon!.name" 
			class="object-contain h-full mt-4 select-none pointer-events-none"
			draggable="false"
		/>

		<div class="information absolute rounded-lg bg-slate-700 w-full h-full -mt-6 opacity-95 px-4 py-6">
			<div class="flex flex-col w-full items-center justify-center">
				<div class="flex ">
					<span class="font-sans text-white font-light">Height:</span>
					<span class="font-sans text-white font-medium  ml-2">{{ (pokemon!.height / 10).toFixed(2) }}m</span>
				</div>
				<div class="flex">
					<span class="font-sans text-white font-light">Weight:</span>
					<span class="font-sans text-white font-medium ml-2">{{ (pokemon!.weight / 10).toFixed(2) }}kg</span>
				</div>
			</div>

			<div class="flex flex-wrap gap-2 mt-6 w-full items-center justify-center">
				<span v-for="pokemonType in pokemon!.types" :type="pokemonType" 
					class="bg-slate-600 py-2 px-4 text-white font-sans text-sm font-light rounded-full capitalize hover:bg-slate-500">
					{{ pokemonType.type.name }}
				</span>
			</div>

		</div>
	</div>
</template>
<style scoped lang="scss">
	.unrevealed { 
		.information {
			display: none;
		}
	}
</style>
