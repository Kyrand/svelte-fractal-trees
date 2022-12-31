<script>
	import Branch from './Branch.svelte'
	export let branches = []
	export let branchNum = 3
	export let cx = 0.5
	export let cy = 0.5
	export let dx = 0
	export let dy = 1
	export let curveHeight = 600
	export let curveWidth = 400
	export let markers = true
	export let markerSize = 40
	export let treeWidth = 50
	export let markerY = 5
	export let symmetrical = false

	let el, d, dt

	$: {
		let h = curveHeight
		let w = curveWidth
		//d = `M -5 0 Q ${cx * w - 5} ${cy * h} ${dx * w} ${dy * h} `
		dt = `M -${treeWidth / 2} 0 Q ${cx * w - treeWidth / 4} ${cy * h} ${dx * w} ${dy * h} Q ${
			cx * w + treeWidth / 4
		} ${cy * h} ${treeWidth / 2} 0 Z`
		d = `M 0 0 Q ${cx * w} ${cy * h} ${dx * w} ${dy * h}`
		el = el
	}

	$: branchTransform = (b) => {
		if (el) {
			let l = el.getTotalLength()
			let p = el.getPointAtLength(b.bp * l)
			let x = p.x
			let y = p.y
			let transform = `translate(${x}, ${y}) rotate(${b.rot}) scale(${b.scaleFac})`
			return transform
		}
	}
</script>

<g transform={`translate(0,0)`} class="tree">
	<!-- <text class="branch">Branch number {branches}</text> -->
	<!-- <path bind:this={el} d={`M 0 0 Q 100 200 0 400} stroke="black" fill="transparent" /> -->
	<path d={dt} stroke="brown" fill="brown" stroke-width={2} />
	<path bind:this={el} {d} stroke="brown" fill="none" stroke-width={1} />

	{#each branches as b, i}
		<g class="branch" id={`${i}`} transform={branchTransform(b)}>
			<Branch {...b} {branchNum} {branches} {markers} {markerSize} {markerY} />
		</g>
	{/each}
</g>

<style>
	.tree {
		pointer-events: none;
	}
</style>
