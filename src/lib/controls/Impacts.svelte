<script>
	import { onMount, tick } from 'svelte'
    import Impact from '$lib/controls/Impact.svelte';
	let impacts

	export let map
	export let position = "top-left"

    const IMPACTS = [
        'Climate_and_Ozone',
        'Respiratory_Health',
        'Cardiovascular_Health',
        'ER_visits_and_Hospital_Admissions',
        'Wheat_Yields',
        'Rice_Yields',
        'Maize_Yields',
        'Soy_Yields',
        'Mortality_and_Crop_Valuation',
        'Labor_Impacts'
    ]

	/**
	 * Component that holds the title of the map
	 */

	class Impacts {
		onAdd(map) {
			this.map = map
			this.container = impacts
			return this.container
		}
		onRemove() {
			this.container.parentNode.removeChild(this.container)
			this.map = undefined
		}
	}
	export const control = new Impacts()

    function onStyleLoad() {
        map.addLayer({
            id: 'methane-impact',
            type: 'fill',
            source: 'composite',
            "source-layer": "country_boundaries",
            paint: {
                "fill-color": [
                    'interpolate',
                    ['linear'],
                    ['number', ['feature-state', 'impact'], 0],
                    0,
                    'rgba(228, 226, 210, 0.4)',
                    0.2,
                    'rgba(196,215,183, 0.4)',
                    0.4,
                    'rgba(152, 199, 143, 0.4)',
                    1,
                    'rgba(61, 166, 63, 0.4)',
                ]
            }
        });
    }

	onMount(async () => {
		await tick()
        map.on('style.load', onStyleLoad)
		map.addControl(control, position);
	})
</script>

<div bind:this={impacts} class="mapboxgl-ctrl control">
    <ul>
        {#each IMPACTS as impact}
            <Impact {impact} {map}></Impact>
        {/each}
    </ul>
</div>
