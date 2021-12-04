<script>
	import MethaneMap from '$lib/MethaneMap.svelte';
	import '../app.css';
</script>

<svelte:head>
	<title>Home</title>
</svelte:head>

<section>
	<MethaneMap/>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 1;
	}
</style>
