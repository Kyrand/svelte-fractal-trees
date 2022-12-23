<script>
	import { getContext } from 'svelte'
	import { interpolate } from 'd3-interpolate'
	import { tweened } from 'svelte/motion'
	import { cubicInOut, sineInOut } from 'svelte/easing'
	const { xGet, xScale, yScale } = getContext('LayerCake')

	export let t = 1.0
	export let duration = 1000
	export let tweenedT = 1.0

	const tweenedDimensions = tweened({ t }, { duration, interpolate: interpolate })

	function setTween(data) {
		tweenedDimensions.set(data, { duration: duration })
	}

	$: setTween({ t })
	$: console.log('t: ', $tweenedDimensions.t)
	$: tweenedT = $tweenedDimensions.t
</script>
