<script>
	import { getContext } from 'svelte'
	import Bar from './Bar.svelte'

	const { custom, config, width, height, data, xGet, yGet, xScale, yScale, extents } =
		getContext('LayerCake')

	$: colorScale = $custom.colorScale
</script>

<div class="index">
	{#each $data as d, i}
		<div
			class="index__label"
			style={`--background-color: ${colorScale[i % colorScale.length]}; --font-size: ${0.9}em;`}
		>
			{d.name}
		</div>
	{/each}
</div>

<style>
	.index {
		display: flex;
		flex-direction: column;
		gap: calc(0.3 * var(--font-size));
	}
	.index__label {
		position: relative;
		line-height: 1;
		font-size: var(--font-size);
		padding-left: calc(0.1 * var(--font-size));

		/* margin-left: 20px; */
	}
	.index__label:before {
		background-color: var(--background-color);
		position: absolute;
		left: calc(-1 * var(--font-size));
		content: '';
		height: var(--font-size);
		width: var(--font-size);
		/* margin-bottom: 15px; */
		/* border: 1px solid black; */
	}
</style>
