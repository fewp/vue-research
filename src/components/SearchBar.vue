<script lang="ts">
export default {
	name: 'SearchBar',
	props: {
		pokemon: Object
	}
};
</script>

<template>
<input
    class="px-8 py-3 rounded-full w-full mt-6 font-sans"
    type="text"
    placeholder="Search for a pokemon..."
/>
</template>
