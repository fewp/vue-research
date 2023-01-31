<script setup lang="ts">
import axios from 'axios';
import { ref } from 'vue';
import PokemonCard from '../components/PokemonCard.vue';
import SearchBar from '../components/SearchBar.vue';

const pokemons = ref([]);

const fetchPokemons = async () => {
	try {
		const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=20');

		response.data.results.forEach(async (element: any) => {
			const pokemon = await axios.get(element.url);
			pokemons.value.push(pokemon.data);
		});
	} catch (err) {
		console.log(err);
	}
};

fetchPokemons();

</script>

<template>
	<div class="flex items-center flex-col container mx-auto justify-center pt-32">
		<div class="flex items-center flex-col w-50 justify-center">
			<h1 class="text-white font-sans text-4xl font-semibold">Welcome to the Pokedex!</h1>
			<p class="text-slate-400 font-sans text-xl font-light mt-1">Click on a card to reveal the pokemon!</p>
			<SearchBar />
		</div>
		<div class="w-full grid grid-cols-6 gap-4 mt-16">
      <PokemonCard  v-for="pokemon in pokemons" :pokemon="pokemon" />
		</div>
	</div>
</template>
