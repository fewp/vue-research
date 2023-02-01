<script lang="ts">
import type { PokemonType } from '@/types';

export default {
	name: 'PokemonTypesFilter',
	props: {
		types: Array<PokemonType>,
	},
	methods: {
		filterByType (event: any) {
			const type = event.target.innerText.toLowerCase();
			const filter = document.querySelector(`.filter.${type}`);
			filter?.classList.toggle('active');

			const activeFilters = document.querySelectorAll('.filter.active');
			const pokemons = document.querySelectorAll('.pokemon-card');

			if (activeFilters.length < 1) {
				pokemons.forEach((pokemon: any) => {
					pokemon.classList.remove('hidden');
				});
				return;
			}

			let filterClasses: string = "";

			activeFilters.forEach((activeFilter: any) => {
				filterClasses += "." + activeFilter.innerText.toLowerCase();
			});

			const filteredPokemon = document.querySelectorAll(`.pokemon-card${filterClasses}`);

			pokemons.forEach((pokemon: any) => {
				pokemon.classList.add('hidden');
			});

			filteredPokemon.forEach((pokemonType: any) => {
				pokemonType.classList.remove('hidden');
			});
			
		}
	}
};

</script>

<template>
	<span v-for="pokemonType in types" :type="pokemonType" 
		@click="filterByType($event)"
		class=" bg-slate-600 py-3 px-6 text-white font-sans font-light rounded-full capitalize hover:bg-slate-500 filter "
		:class="pokemonType.name"
	>
		{{ pokemonType.name }}
	</span>
</template>

<style scoped lang="scss">
	.filter.active {
		@apply bg-orange-400;
	}
</style>