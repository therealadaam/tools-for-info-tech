<script>
	import Nav from '../components/nav.svelte';
</script>

<Nav />
<div class="p-8 max-w-6xl mx-auto">
	<slot />
</div>

<style>
	@tailwind base;
	@tailwind components;
	@tailwind utilities;
	/* :global(body) {
		background-color: #1d3040;
		color: #bfc2c7;
	} */
	/*
	:global(a) {
		color: #bfc2c7;
	}
	:global(p) {
		width: auto;
		color: darkolivegreen;
		/* font-size: 15; */
	/* } */
	/* :global(.div) {
		width: auto;
		min-width: 5em;
		border: 0.25em solid darkslategray;
		padding: 1em;
		margin: 1em;
		background-color: darkgray;
	} */
</style>
