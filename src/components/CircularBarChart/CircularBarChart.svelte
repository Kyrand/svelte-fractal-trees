<script>
	import { LayerCake, Svg, Html } from 'layercake'
	import { schemeCategory10 } from 'd3-scale-chromatic'

	import Bars from './Bars.svelte'
	import Intro from './Intro.svelte'
	import Legend from './Legend.svelte'

	export let data = []
	export let xKey = 'value'
	export let nameKey = 'name'
	export let hubFactor = 0.1
	export let labelPadding = 0.1
	export let text = null
	export let colorScale = schemeCategory10
	export let legend = true
	export let labels = false
	export let legendScale = 0.8

	$: leftPadding = legend ? 150 : 20
</script>

<div class="chart-container">
	<LayerCake
		padding={{ top: 20, right: 20, bottom: 20, left: leftPadding }}
		x={xKey}
		{data}
		xDomain={[0, null]}
		xRange={({ width, height }) => [0, ((1 - hubFactor - labelPadding) * width) / 2]}
		custom={{ colorScale }}
	>
		<Svg>
			<Bars {hubFactor} {nameKey} {labels} />
		</Svg>
		{#if text}
			<Html>
				<Intro {text} {hubFactor} />
			</Html>
		{/if}
		{#if legend}
			<Html>
				<div class="legend__wrapper" style={`scale: ${legendScale};`}>
					<Legend />
				</div>
			</Html>
		{/if}
	</LayerCake>
</div>

<style>
	.chart-container {
		width: 100%;
		height: 100%;
	}

	.legend__wrapper {
		display: inline-block;
		transform-origin: top left;
	}
</style>
