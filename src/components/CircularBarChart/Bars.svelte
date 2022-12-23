<script>
	import { getContext } from 'svelte'
	import Bar from './Bar.svelte'
	import { schemeCategory10 } from 'd3-scale-chromatic'

	const { custom, config, width, height, data, xGet, yGet, xScale, yScale, extents } =
		getContext('LayerCake')

	export let hubFactor = 0.2
	export let nameKey = 'name'
	export let labels = false

	$: hubRadius = hubFactor * $width * 0.5
	$: console.log(
		'Bar-data: ',
		$data.map((d) => $xGet(d))
	)
	$: colorScale = $custom.colorScale

	// tan t/2 = (bw/2) / hR
	$: theta = (Math.PI * 2) / $data.length
	$: thetaDeg = theta * (180 / Math.PI)
	$: barWidth = 2 * Math.tan(theta / 2) * hubRadius
	$: console.log(theta, thetaDeg, barWidth, $config.x)
	$: console.log($xScale.range(), $xScale.domain())
</script>

<g transform="translate({$width / 2}, {$height / 2})">
	<circle class="hub" cx="0" cy="0" r={hubRadius} />
	{#each $data as d, i (d.name)}
		<Bar
			label={labels && d.name}
			theta={180 + thetaDeg * i}
			x={-barWidth / 2}
			y={hubRadius}
			width={barWidth}
			height={$xGet(d)}
			fill={colorScale[i % colorScale.length]}
		/>
	{/each}
</g>

<style>
	.bar rect {
		fill: none;
		stroke-width: 1px;
		stroke: blue;
	}

	circle.hub {
		fill: none;
		stroke-width: 1px;
		stroke: black;
	}
</style>
