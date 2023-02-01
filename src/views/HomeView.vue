<script setup lang="ts">
import PokemonCard from "@/components/PokemonCard.vue";
import PokemonByTypesFilter from "@/components/PokemonTypesFilter.vue";
import SearchBar from "@/components/SearchBar.vue";
import SkeletonCard from "@/components/SkeletonCard.vue";
import type { PokemonType } from "@/types";
import axios from "axios";
import { ref } from "vue";

const loadValues = [
  {
    display: "20",
    data: 20,
  },
  {
    display: "151",
    data: 151,
  },
  {
    display: "251",
    data: 251,
  },
  {
    display: "386",
    data: 386,
  },
  {
    display: "All",
    data: 20000,
  },
];

const pokemons = ref<PokemonType[]>([]);
const pokemonTypes = ref<PokemonType[]>([]);
const loadAmount = ref<number>(loadValues[0].data);
const offset = ref<number>(0);
const isLoading = ref<boolean>(true);

const loadPokemonTypes = () => {
  pokemonTypes.value = [
    ...pokemons.value.reduce((acc: any, pokemon: any) => {
      pokemon.types.forEach((type: any) => {
        acc.add(type.type);
      });
      return acc;
    }, new Set()), //o set não permite repetições, então voce consegue otimizar o código de forma a não precisar mais fazer aquele '.some'
  ];
};

const fetchPokemons = async () => {
  try {
    const baseURL: string = "https://pokeapi.co/api/v2/pokemon";
    let url: string = baseURL;
    if (offset.value === 0) url += "?limit=" + loadAmount.value;
    else
      url +=
        "?limit=" +
        (pokemons.value.length - offset.value) +
        "&offset=" +
        offset.value;

    const response = await axios.get(url);

    pokemons.value = await response.data.results.reduce(
      async (acc, element: any) => {
        const pokemon = await axios.get(element.url);
        const pokemonObject: any = pokemon.data;
        return [...(await acc), pokemonObject];
      },
      []
    );
  } catch (err) {
    console.log(err);
  }
};

watch(
  () => pokemons.value,
  () => loadPokemonTypes()
);

fetchPokemons();

const changeLoadAmount = (event: any) => {
  const requestedAmount: number = parseInt(
    event.target.getAttribute("data"),
    10
  );
  if (requestedAmount === loadAmount.value) return;

  isLoading.value = true;

  if (requestedAmount < loadAmount.value) {
    pokemons.value = pokemons.value.slice(0, requestedAmount);
    loadPokemonTypes();
    loadAmount.value = requestedAmount;
    isLoading.value = false;
    return;
  } else {
    loadAmount.value = requestedAmount;
  }

  pokemons.value = [];
  pokemonTypes.value = [];
  fetchPokemons();
};
</script>
<template>
  <div class="animated-background">
    <div
      class="flex items-center flex-col container mx-auto justify-center py-16"
    >
      <div
        class="flex items-center flex-col w-full lg:w-1/2 justify-center lg:pt-16 px-4"
      >
        <div
          class="flex items-center justify-center gap-2 mb-8 flex-wrap select-none"
        >
          <PokemonByTypesFilter :types="pokemonTypes" v-if="!isLoading" />
          <span
            class="h-9 w-24 rounded-full loading"
            v-for="n in 5"
            v-else-if="isLoading"
          ></span>
        </div>

        <h1
          class="text-white font-sans text-4xl font-semibold mt-8 text-center"
        >
          Welcome to the <span class="text-orange-400 font-bold">Pokedex</span>!
        </h1>
        <p class="text-slate-400 font-sans text-xl font-light my-2 text-center">
          Click on a card to reveal more information about the pokemon! You can
          also search by name or filter by one of the
          <span class="text-slate-200 font-medium">
            {{ pokemonTypes.length }}
          </span>
          loaded types!
        </p>
        <SearchBar />
      </div>
    </div>
  </div>
  <div
    class="flex items-start flex-col text-lg container mx-auto justify-center px-4"
  >
    <div v-if="!isLoading" class="flex w-full flex-col">
      <div
        class="w-full flex flex-col lg:flex-row justify-between items-center py-8"
      >
        <p class="text-slate-900 font-sans">
          Showing {{ pokemons.length }} pokemon
        </p>
        <div
          class="flex justify-center lg:justify-between items-center gap-2 mt-4 lg:mt-0 flex-wrap"
        >
          <button
            v-for="value in loadValues"
            class="px-6 py-3 rounded-full font-sans text-sm font-medium hover:bg-slate-600 hover:text-white"
            :data="value.data"
            :class="[
              value.data === loadAmount
                ? 'bg-orange-300 text-slate-800'
                : 'bg-slate-200 text-slate-600',
            ]"
            @click="changeLoadAmount"
          >
            {{ value.data === loadAmount ? "Showing" : "Show" }}
            {{ value.display }}
          </button>
        </div>
      </div>
      <div
        class="w-full grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-6 gap-4 pb-8"
      >
        <PokemonCard v-for="pokemon in pokemons" :pokemon="pokemon" />
      </div>
    </div>
    <div v-else-if="isLoading" class="flex w-full flex-col">
      <div class="w-full flex justify-between items-center py-8">
        <span class="h-12 w-1/3 rounded-full loading hidden lg:flex"></span>

        <div class="flex justify-between items-center space-x-2">
          <span class="h-12 w-32 rounded-full loading" v-for="n in 4"></span>
        </div>
      </div>
      <div
        class="w-full grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-6 gap-4 pb-8"
      >
        <SkeletonCard v-for="n in 12" />
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.animated-background {
  animation: GradientBackground 25000ms infinite linear reverse;
  background: linear-gradient(90deg, #0f172a, #1e293b, #0f172a) 0 0 / 400% 100%;
}
</style>
