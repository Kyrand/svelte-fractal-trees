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
	let scale = 0.7
	let duration = 3000
	let _branchCount = 3,
		_branchNum = 5,
		_markerSize = 15
	let branchNum = 5 //4
	let branchCount = 3 //6
	let t = 1.0
	let scaleFac = 0.5
	let markers = true
	let markerSize = 10
	let symmetrical = false
	let treeHand = 0
	let auto = false
	let refBranchRot = 45

	// tweening
	//let bv = makeRandomTrunk()
	let bv = tweened(makeRandomTrunk(), { duration })
	let intervalMBR = () => {
		if (auto) {
			randomizeTree()
		}
	}
	let clear
	$: {
		clearInterval(clear)
		clear = setInterval(intervalMBR, 8000)
	}

	let branches
	initBranches()
	function initBranches() {
		treeHand = Math.random() < 0.5 ? 0 : 1
		branches = tweened(
			[
				...Array(branchCount)
					.fill(0)
					.map((_, i) => makeRandomBranch(i))
			],
			{ duration }
		)
	}

	randomizeTree()
	console.log(branches)
	//let bv = branches[0]
	// functions
	function rv(min, max) {
		return min + Math.random() * (max - min)
	}

	function makeRandomTrunk() {
		return {
			cx: rv(-0.4, 0.4),
			cy: rv(0.3, 0.7),
			dx: rv(-0.4, 0.4),
			dy: rv(0.9, 1),
			markerY: rv(2, 10)
		}
	}

	function makeRandomBranch(i) {
		let rot = rv(-25, 25)
		// parity = branchTypes[Math.floor(Math.random() * branchTypes.length)]
		if ((i + treeHand) % 2) {
			rot = refBranchRot + rot
		} else {
			rot = -refBranchRot + rot
		}
		return {
			scaleFac: rv(0.6, 0.7),
			rot,
			cx: rv(-0.4, 0.4),
			cy: rv(0.3, 0.6),
			dx: rv(-0.4, 0.4),
			dy: rv(0.4, 1),
			bp: rv(0.5, 0.9),
			markerRot: rv(-90, 90)
		}
	}

	function makeRandomBranchFull() {
		return {
			scaleFac: rv(0.6, 0.7),
			rot: rv(-135, 135),
			cx: rv(-1, 1),
			cy: rv(0, 1),
			dx: rv(-1, 1),
			dy: rv(0, 1),
			bp: rv(0.25, 0.75)
		}
	}

	function randomizeTree() {
		markerSize = _markerSize
		branchNum = _branchNum
		treeHand = Math.random() < 0.5 ? 0 : 1
		bv.set(makeRandomTrunk())
		branches.set([
			...Array(branchCount)
				.fill(0)
				.map((d, i) => makeRandomBranch(i))
		])
	}

	function selectTreeType(e) {
		switch (e.target.value) {
			case 'small':
				branchCount = 3
				_branchNum = 5
				_markerSize = 10
				duration = 3000
				break
			case 'medium':
				console.log('Here!!!')
				branchCount = 4
				_branchNum = 6
				_markerSize = 10
				duration = 0
				auto = false
				break
		}
		initBranches()
	}
	// $: scaleFac = _scaleFac / 100
	// console logs
	$: console.log('tweenedT: ', tweenedT)
	$: console.log('duration: ', duration)
	$: console.log('scaleFac ', scaleFac)
</script>

<div class="_container">
	<h2>Fractal trees with Svelte (and transitions)</h2>
	<p>
		A little generative art, using some self-referencing branch components to make fractal trees.<br
		/>
		You can select small trees, which will transition smoothly, or large ones, which will take a few
		seconds to generate. Set the auto-switch to see update the trees automatically every 10s.
	</p>
	<div class="app__wrapper">
		<div class="ui">
			<button on:click={randomizeTree}>New tree</button>
			<Switch bind:flag={markers} label="Show leaves" />
			<fieldset on:change={selectTreeType}>
				<legend>Branch number</legend>
				<label for="small">
					<input type="radio" id="small" name="size" value="small" checked />
					Small (transitions)
				</label>
				<label for="medium">
					<input type="radio" id="medium" name="size" value="medium" />
					Large (slow)
				</label>
			</fieldset>
			<Switch bind:flag={auto} label="Auto change" />
			<details>
				<summary>Controls</summary>

				<!-- <Switch bind:flag={stackLines} label="Stack the lines" id="stackLines" />
			<Switch bind:flag={normalizeAxes} label="Dynamic y axis" id="normAxes" /> -->
				<Range bind:value={duration} min={0} max={5000} label="Transition duration (ms)" />
				<Range bind:value={markerSize} min={10} max={200} label="Leaf size" />
				<!-- <RangeF bind:value={bv.scaleFac} label="Scale factor" /> -->
				<RangeF bind:value={$bv.cx} label="Curve mid x" min={-1.0} max={1.0} />
				<RangeF bind:value={$bv.cy} label="Curve mid y" />
				<RangeF bind:value={$bv.dx} label="Curve end x" min={-1.0} max={1.0} />
				<RangeF bind:value={$bv.dy} label="Curve end y" />
				<RangeF bind:value={scale} label="Tree scale" />
			</details>
		</div>
		<div class="chart-container">
			<svg class="tree">
				<g transform={`translate(400, 800) rotate(180) scale(${scale})`}>
					<Tree {...$bv} branches={$branches} {branchNum} {markers} {markerSize} />
				</g>
			</svg>
		</div>
	</div>
</div>

<style>
	/*
    The wrapper div needs to have an explicit width and height in CSS.
    The point being it needs dimensions since the <LayerCake> element will
    expand to fill it.
  */
	.app__wrapper {
		display: flex;
		flex-direction: row;
		gap: 4em;
	}
	.chart-container {
		width: 800px;
		height: 800px;
	}
	.ui {
		min-width: 220px;
	}

	.tree {
		width: 100%;
		height: 100%;
		overflow: visible;
	}
</style>
