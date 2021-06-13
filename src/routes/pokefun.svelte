<script context="module">
	export async function load({ page }) {
		const url = 'https://pokeapi.co/api/v2/pokemon?limit=100';
		const res = await fetch(url);
		const data = await res.json();
		const loadedPokemon = data.results.map((data, index) => {
			return {
				name: data.name,
				id: index + 1,
				image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${
					index + 1
				}.png`
			};
		});
		return { props: { pokemon: loadedPokemon } };
	}
</script>

<script>
	import PokemanCard from '../components/pokemanCard.svelte';
	export let pokemon;

	let searchTerm = '';
	let filteredPokemon = [];

	$: {
		if (searchTerm) {
			filteredPokemon = pokemon.filter((pokeman) =>
				pokeman.name.toLowerCase().includes(searchTerm.toLocaleLowerCase())
			);
		} else {
			filteredPokemon = [...pokemon];
		}
	}
</script>

<h1 class="text-4xl text-center my-8 uppercase">Svelte Kit Pokedex</h1>

<input
	class="w-full rounded-md text-lg p-4 border-2 border-gray-200"
	type="text"
	placeholder="Search Pokemon"
	bind:value={searchTerm}
/>
<!-- <button on:click={showPokemon}>Show Pokemon</button> -->
<div class="py-4 grid gap-4 md:grid-cols-3 grid-cols-1">
	{#each filteredPokemon as pokeman}
		<PokemanCard {pokeman} />
	{/each}
</div>
