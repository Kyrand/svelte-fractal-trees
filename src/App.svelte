<script>
	import { LayerCake, Svg, flatten } from 'layercake'
	import { stack, max, extent } from 'd3'
	import { scaleOrdinal } from 'd3-scale'
	import { format, precisionFixed } from 'd3-format'
	import { timeParse, timeFormat } from 'd3-time-format'
	import { schemeTableau10, rollup, group } from 'd3'
	import { tweened } from 'svelte/motion'
	import { cubicOut } from 'svelte/easing'

	import Tree from './components/Tree/Tree.svelte'

	import Switch from './components/Switch.svelte'
	import Range from './components/Range.svelte'
	import RangeF from './components/RangeF.svelte'
	import Tweener from './components/layercake/Tweener.svelte'
	import Legend from './components/Legend.svelte'

	let tweenedT = 1
	let scale = 1
	let duration = 3000
	let branchNum = 5
	let branchCount = 4
	let t = 1.0
	let scaleFac = 0.5
	let cx = rv(-0.2, 0.2),
		cy = rv(0.3, 0.7),
		dx = rv(-0.2, 0.2),
		dy = rv(0.8, 1),
		bp = 0.5,
		rot = 45
	let markers = true

	// tweening
	let bv = { scaleFac, cx, cy, dx, dy, bp, rot }
	let branches
	randomizeBranches()
	console.log(branches)
	//let bv = branches[0]
	// functions
	function rv(min, max) {
		return min + Math.random() * (max - min)
	}

	function makeRandomBranch() {
		return {
			scaleFac: rv(0.5, 0.7),
			rot: rv(-135, 135),
			cx: rv(-1, 1),
			cy: rv(0, 1),
			dx: rv(-1, 1),
			dy: rv(0, 1),
			bp: rv(0.5, 1)
		}
	}

	function randomizeBranches() {
		branches = [
			...Array(branchCount)
				.fill(0)
				.map(() => makeRandomBranch())
		]
	}
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
			<RangeF bind:value={bv.scaleFac} label="Scale factor" />
			<RangeF bind:value={bv.cx} label="Curve mid x" min={-1.0} max={1.0} />
			<RangeF bind:value={bv.cy} label="Curve mid y" />
			<RangeF bind:value={bv.dx} label="Curve end x" min={-1.0} max={1.0} />
			<RangeF bind:value={bv.dy} label="Curve end y" />
			<Switch bind:flag={markers} label="Show leaves" />
			<button on:click={randomizeBranches}>Randomize branches</button>
		</div>
		<div class="chart-container">
			<LayerCake>
				<Svg>
					<g transform={`translate(300, 600) rotate(180) scale(${scale})`}>
						<Tree {...bv} {branches} {branchNum} {markers} />
					</g>
				</Svg>
			</LayerCake>
		</div>
	</div>
</div>

<style>
	/*
    The wrapper div needs to have an explicit width and height in CSS.
					<g transform="translate(300, 600) rotate(180)">
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
