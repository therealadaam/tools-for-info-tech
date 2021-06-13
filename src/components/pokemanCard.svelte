<script>
	export let pokeman;
</script>

<a class="pokeCard" href={`/pokemon/${pokeman.id}`}>
	<img class="h-40 w-40" src={pokeman.image} alt={pokeman.name} />
	<h2 class="text-2xl uppercase">{pokeman.id}. {pokeman.name}</h2>
</a>
