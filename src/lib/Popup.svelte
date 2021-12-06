
<script>
	import { onMount, tick } from 'svelte'

	export let map

    function getDescriptionFromEvent (e) {
        let description = `
            <div>${e.features[0].properties.name_en}</div>
        `
        if (e.features[0].state.impact && e.features[0].state.impact.toFixed(2) > 0) {
            description += `
                <div>Impact: $${e.features[0].state.impact.toFixed(2)}</div>
            `
        }
        return description
    }

    /**
     * https://docs.mapbox.com/mapbox-gl-js/example/popup-on-hover/
     */
    function setupPopup () {
        // Create a popup, but don't add it to the map yet.
        const popup = new mapboxgl.Popup({
            closeButton: false,
            closeOnClick: false
        });
        
        map.on('mouseenter', 'methane-impact', (e) => {
            // Change the cursor style as a UI indicator.
            map.getCanvas().style.cursor = 'pointer';
            
            // Copy coordinates array.
            const coordinates = e.lngLat
            const description = getDescriptionFromEvent(e)
            
            // Populate the popup and set its coordinates
            // based on the feature found.
            popup.setLngLat(coordinates).setHTML(description).addTo(map);
        });
        map.on('mousemove', 'methane-impact', (e) => {
            const coordinates = e.lngLat
            const description = getDescriptionFromEvent(e)
            popup.setLngLat(coordinates).setHTML(description)
        })
        
        map.on('mouseleave', 'methane-impact', () => {
            map.getCanvas().style.cursor = '';
            popup.remove();
        });
    }

	onMount(async () => {
		await tick()
        map.on('load', setupPopup)
	})
</script>
