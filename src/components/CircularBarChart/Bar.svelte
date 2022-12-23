<script>
	import { getContext } from 'svelte'
	import { interpolate } from 'd3-interpolate'
	import { tweened } from 'svelte/motion'
	import { cubicInOut, sineInOut } from 'svelte/easing'

	export let title = ''
	export let height = 10
	export let width = 10
	export let x = 0
	export let y = 0
	export let theta = 0
	export let duration = 1000
	export let barStyle = {}
	export let barLabelPad = 5
	export let fill = 'blue'
	export let label = ''

	const { xGet, xScale, yScale } = getContext('LayerCake')
	const tweenedDimensions = tweened(
		{
			x,
			y,
			theta,
			width,
			height
		},
		{
			duration,
			interpolate: interpolate
			// easing: sine// cubicInOut
		}
	)

	function setTween(data) {
		tweenedDimensions.set(data)
	}
	$: setTween({
		x,
		y,
		theta,
		width,
		height
	})

	$: maxX = $xScale.range()[1]
	// $: getLabelAttrs = (d) => {
	// 	let x = $tweenedDimensions.width
	// 	let y = $tweenedDimensions.height / 2
	// 	if (maxX - x > barLabelThresh) {
	// 		return {
	// 			x: x + barLabelPad,
	// 			'text-anchor': 'start',
	// 			y: y,
	// 			class: 'bar-label',
	// 			fill: fill
	// 		}
	// 	} else {
	// 		return {
	// 			x: x - barLabelPad,
	// 			'text-anchor': 'end',
	// 			y: y,
	// 			class: 'bar-label inverse'
	// 		}
	// 	}
	// }
</script>

<g
	class="bar"
	transform={`rotate(${$tweenedDimensions.theta})translate(${$tweenedDimensions.x}, ${$tweenedDimensions.y})`}
>
	<rect width={$tweenedDimensions.width} height={$tweenedDimensions.height} {fill} />
	{#if label}
		<text
			class="bar__label"
			text-anchor="start"
			_dy="-0.35em"
			transform="rotate(90)translate({$tweenedDimensions.height + barLabelPad},0)">{label}</text
		>
	{/if}
</g>

<!-- <g class="bar" transform={`translate(0,${$tweenedDimensions.y})`}>
	<text
		class="tick"
		x={$tweenedDimensions.x - 8}
		y={$tweenedDimensions.height / 2}
		dominant-baseline="middle"
		text-anchor="end"
	>
		{title}
	</text>
	<rect
		style={barStyle}
		x={$tweenedDimensions.x}
		y={0}
		height={$tweenedDimensions.height}
		width={$tweenedDimensions.width}
		{fill}
	/>
	<text alignment-baseline="middle" {...getLabelAttrs(d)}>
		{d.value + '%'}
	</text>
</g> -->
<style>
	.tick {
		font-size: 0.625em;
		font-weight: bold;
		/* font-weight: 200; */
	}

	text.bar__label {
		font-size: 0.8em;
		/* font-weight: bold; */
	}

	text.bar-label.inverse {
		fill: #efefef;
	}
	/* .bar text {
    fill: #1b1e23;
    font-style: italic;
    font-size: 0.9em;
    font-family: 'Source Serif Pro', Iowan Old Style, Apple Garamond,
      Palatino Linotype, Times New Roman, 'Droid Serif', Times, serif,
      Apple Color Emoji, Segoe UI Emoji, Segoe UI Symbol;
  } */
</style>
