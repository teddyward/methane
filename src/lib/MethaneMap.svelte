<script>
	import { onMount } from 'svelte';
    import Impacts from '$lib/controls/Impacts.svelte';
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
        <Impacts {map}></Impacts>
    </slot>
</div>

<style>
	#map {
		height: 100vh;
		width: 100vw;
	}
    #map :global(.control) {
        background-color: rgba(256, 256, 256, 0.7);
        margin: 0;
        padding: 5px;
        z-index: 2;
    }
</style>
