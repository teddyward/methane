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
            const featureKey = {
                source: "composite",
                "sourceLayer": "country_boundaries",
                "id": countryFeature[0].id
            }
            const oldValue = map.getFeatureState(featureKey).impact || 0
            let newValue = countryValue
            if (isActive) {
                newValue = countryValue + oldValue
            } else {
                newValue = oldValue - countryValue
            }
            map.setFeatureState(featureKey, {impact: newValue})
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
