<script>
	import { onMount, tick } from 'svelte'
    import Impact from '$lib/controls/Impact.svelte';
    import impactsData from '$lib/impacts.json'
    import {impactData, countries} from '$lib/controls/impactStores'
import { debug } from 'svelte/internal';
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
        console.log(impactsData)
        map.addLayer({
            id: 'methane-impact',
            type: 'fill',
            source: 'composite',
            "source-layer": "country_boundaries",
            paint: {
                "fill-color": [
                    "case",
                    ["in", ["get", "worldview"], ["literal", ["all", "JP"]]],
                    // for countries in JP worldview, style as such
                    [
                        'interpolate',
                        ['linear'],
                        ['number', ['feature-state', 'impact'], 0],
                        0,
                        'rgba(228, 226, 210, 0.3)',
                        100,
                        'rgba(220, 220, 205, 0.3)',
                        1000,
                        'rgba(196,215,183, 0.4)',
                        10000,
                        'rgba(152, 199, 143, 0.5)',
                        100000,
                        'rgba(61, 166, 63, 0.6)',
                    ],
                    // otherwise: fallback to transparent
                    'rgba(256, 256, 256, 0.0)',
                ]
            }
        });
    }

	onMount(async () => {
		await tick()
        map.on('style.load', onStyleLoad)
		map.addControl(control, position);
        impactData.set(impactsData.impacts)
        countries.set(Array.from(
            new Set($impactData.map((impact) => impact.Country))
        ))
	})
</script>

<div bind:this={impacts} class="mapboxgl-ctrl control">
    <div>Click one or more impacts to visualize:</div>
    <ul>
        {#each IMPACTS as impact}
            <Impact {impact} {map}></Impact>
        {/each}
    </ul>
</div>
