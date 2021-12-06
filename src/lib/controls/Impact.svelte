<script>
    export let impact = '';
    export let map = {};

	let isActive = false;
    let displayName;
    $: {
        if (impact.replaceAll) {
            displayName = `${impact}`.replaceAll('_', ' ')
        }
    }

    function colorImpact() {
        isActive = !isActive;
        const russia = map.querySourceFeatures('composite', {
            'sourceLayer': "country_boundaries",
            filter: ['in', ["get", "name_en"], ["literal", ["Russia"]]]
        });
        
        map.setFeatureState(
            {source: "composite", "sourceLayer": "country_boundaries", "id": russia[0].id},
            {impact: .5}
        )
    }
</script>

<li 
    on:click="{() => colorImpact()}"
    class="mapboxgl-ctrl impact-name"
    class:active="{isActive}"
>
    {displayName}
</li>

<style>
    .impact-name {
        cursor: pointer !important;
        color: grey;
    }
    .active {
        color: black;
    }
</style>
