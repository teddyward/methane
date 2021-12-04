<script>
	import { onMount } from 'svelte';
    import Title from '$lib/controls/Title.svelte';

    let map;
	
    onMount(() => {
        mapboxgl.accessToken = 'pk.eyJ1IjoidGhyb3dhd2F5ODY3NTMwOSIsImEiOiJja2Q3bmZ4c2ExZWw0MnlvMmZ2OWR1MHE5In0.lt1pIT7P5njAWbfnucdQmQ';

        map = new mapboxgl.Map({
            container: 'map', // container id
            style: 'mapbox://styles/throwaway8675309/ckwsejsz61zf914mxk2sotkln', // style URL
            center: [0, 16], // starting position [lng, lat]
            zoom: 1.35 // starting zoom
        });
    });
</script>

<div id="map">
    <slot name="controls">
        <Title {map}></Title>
    </slot>
</div>

<style>
	#map {
		height: 100vh;
		width: 100vw;
	}
</style>
