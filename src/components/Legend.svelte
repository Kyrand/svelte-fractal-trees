<script>
	import { getContext } from 'svelte'

	export let flexDirection = 'row'
	export let labels = []
	export let colorScale = []

	//const { custom, config, width, height, data, xGet, yGet, xScale, yScale, extents } =
	//	getContext('LayerCake')

	//$: colorScale = $custom.colorScale
</script>

<div class="legend" style={`flex-direction: ${flexDirection};`}>
	{#each labels as label, i}
		<div
			class="legend__label"
			style={`--background-color: ${colorScale[i % colorScale.length]}; --font-size: ${0.7}em;`}
		>
			{label}
		</div>
	{/each}
</div>

<style>
	.index {
		display: flex;
		flex-direction: column;
		gap: calc(0.3 * var(--font-size));
	}
	.legend__label {
		position: relative;
		line-height: 1;
		font-size: var(--font-size);
		padding-left: calc(0.5 * var(--font-size));
		padding-bottom: calc(0.2 * var(--font-size));

		/* margin-left: 20px; */
	}
	.legend__label:before {
		background-color: var(--background-color);
		position: absolute;
		left: calc(-1 * var(--font-size));
		content: '';
		height: calc(1.2 * var(--font-size));
		width: calc(1.2 * var(--font-size));
		/* margin-bottom: 15px; */
		/* border: 1px solid black; */
	}

	.legend {
		_display: flex;
		_flex-direction: row;
		_flex-wrap: wrap;
		columns: 180px auto;
		_column-gap: 2em;
		row-gap: 0.8em;
		padding: 2em 0;
	}
</style>
