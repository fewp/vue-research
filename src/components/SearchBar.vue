<script lang="ts">
export default {
	name: 'SearchBar',
	props: {
		pokemons: Array
	},
	methods: {
		handleSearch (event: any) {
			const searchQuery = event.target.value.toLowerCase();
			const pokemons = document.querySelectorAll('.pokemon-card');
			const pokemonNames = document.querySelectorAll('.pokemon-card h3');

			pokemons.forEach((pokemon: any) => {
				pokemon.classList.add('hidden');
			});

			pokemonNames.forEach((pokemonName: any) => {
				if (pokemonName.innerText.toLowerCase().includes(searchQuery)) {
					pokemonName.closest('.pokemon-card').classList.remove('hidden');
				}
			});
		}
	},
};

</script>

<template>
<input
    class="px-8 py-3 rounded-full w-full mt-6 font-sans focus:outline-none focus:ring-2 
		focus:ring-orange-500"
    type="text"
    placeholder="Search for a pokemon..."
	@keyup="handleSearch($event)"
/>
</template>

<style scoped lang="scss">
	input {
		transition: box-shadow 200ms ease-in-out;
	}
</style>