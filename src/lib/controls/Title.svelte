<script>
	import { onMount, tick } from 'svelte'
	let title

	export let map
	export let position = "top-right"

	/**
	 * Component that holds the title of the map
	 */

	class Title {
		onAdd(map) {
			this.map = map
			this.container = title
			return this.container
		}
		onRemove() {
			this.container.parentNode.removeChild(this.container)
			this.map = undefined
		}
	}
	export const control = new Title()

	onMount(async () => {
		await tick()
		map.addControl(control, position);
	})
</script>

<div bind:this={title}>
    Methane Impacts and Mitigations
</div>
