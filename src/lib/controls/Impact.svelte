<script>
    export let impact = '';
    export let map = {};
    import {impactData, countries} from '$lib/controls/impactStores'

	let isActive = false;
    let displayName;
    $: {
        if (impact.replaceAll) {
            displayName = `${impact}`.replaceAll('_', ' ')
        }
    }

    function colorImpact() {
        isActive = !isActive;
        $countries.forEach((country) => {
            const countryFeature = map.querySourceFeatures('composite', {
                'sourceLayer': "country_boundaries",
                filter: ['in', ["get", "name_en"], ["literal", [country]]]
            });
            if (!countryFeature || !countryFeature.length) return
            const countryValue = $impactData.find((impactDatum) => {
                return impactDatum.Country == country && impactDatum.ResponseToCH4 == impact
            }).Value
            map.setFeatureState(
                {source: "composite", "sourceLayer": "country_boundaries", "id": countryFeature[0].id},
                {impact: countryValue}
            )
        })
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
