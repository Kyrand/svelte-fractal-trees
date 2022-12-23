<script>
	import { LayerCake, Svg, flatten } from 'layercake'
	import { stack, max, extent } from 'd3'
	import { scaleOrdinal } from 'd3-scale'
	import { format, precisionFixed } from 'd3-format'
	import { timeParse, timeFormat } from 'd3-time-format'
	import { schemeTableau10, rollup, group } from 'd3'
	import { tweened } from 'svelte/motion'
	import { cubicOut } from 'svelte/easing'

	import Branch from './components/Tree/Branch.svelte'

	import Switch from './components/Switch.svelte'
	import Range from './components/Range.svelte'
	import RangeF from './components/RangeF.svelte'
	import Tweener from './components/layercake/Tweener.svelte'
	import Legend from './components/Legend.svelte'

	let tweenedT = 1
	let duration = 3000
	let branches = 10
	let t = 1.0
	let scaleFac = 0.5
	let cx = 0.5,
		cy = 0.5,
		dx = 0.5,
		dy = 1,
		bp = 0.5

	// tweening
	let bv = { cx, cy, dx, dy, bp }

	// $: scaleFac = _scaleFac / 100
	// console logs
	$: console.log('tweenedT: ', tweenedT)
	$: console.log('duration: ', duration)
	$: console.log('scaleFac ', scaleFac)
	$: console.log('cx ', cx)
</script>

<div class="_container">
	<h2>Recursive trees with growth transitions</h2>
	<h3>A Svelte, LayerCake, D3 production</h3>
	<p>blah...</p>
	<div class="app__wrapper">
		<div class="ui">
			<!-- <Switch bind:flag={stackLines} label="Stack the lines" id="stackLines" />
			<Switch bind:flag={normalizeAxes} label="Dynamic y axis" id="normAxes" /> -->
			<Range bind:value={duration} min={0} max={5000} label="Transition duration (ms)" />
			<RangeF bind:value={scaleFac} label="Scale factor" />
			<RangeF bind:value={bv.cx} label="Curve mid x" />
			<RangeF bind:value={bv.cy} label="Curve mid y" />
			<RangeF bind:value={bv.dx} label="Curve end x" />
			<RangeF bind:value={bv.dy} label="Curve end y" />
			<RangeF bind:value={bv.bp} label="Branch position" />
		</div>
		<div class="chart-container">
			<LayerCake padding={{ top: 0, right: 0, bottom: 0, left: 0 }}>
				<Svg>
					<g transform="translate(300, 600) rotate(180)">
						<Branch
							{branches}
							{scaleFac}
							cx={2 * (bv.cx - 0.5)}
							cy={bv.cy}
							dx={2 * (bv.dx - 0.5)}
							dy={bv.dy}
							bp={bv.bp}
						/>
					</g>
					<Tweener {t} bind:tweenedT {duration} />
				</Svg>
			</LayerCake>
		</div>
	</div>
</div>

<style>
	/*
    The wrapper div needs to have an explicit width and height in CSS.
    It can also be a flexbox child or CSS grid element.
    The point being it needs dimensions since the <LayerCake> element will
    expand to fill it.
  */
	.app__wrapper {
		display: flex;
		flex-direction: row;
		gap: 4em;
	}
	.chart-container {
		width: 600px;
		height: 600px;
	}
	.ui {
		min-width: 220px;
	}
</style>
