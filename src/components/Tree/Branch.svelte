<script>
	export let branchNum = 0
	export let branches = []
	export let scaleFac = 0.5
	export let t = 0.5
	export let curveFac = 0.5
	export let curveHeight = 600
	export let curveWidth = 600
	export let cx = 0.5
	export let cy = 0.5
	export let dx = 0
	export let dy = 1
	export let bp = 0.5
	export let rot = 45
	export let markers = true
	export let markerSize = 40
	export let markerScale = 1
	export let markerY = 5
	export let markerRot = 0
	export let parity = 'symm'

	let markerTransform = 'translate(0,0)'

	let d
	let el, l

	$: {
		let h = curveHeight
		let w = curveWidth
		d = `M 0 0 Q ${cx * w} ${cy * h} ${dx * w} ${dy * h}`
		let endPt = { x: dx * w, y: dy * h }
		markerTransform = `translate(${endPt.x}, ${endPt.y}) rotate(${markerRot}) scale(${markerScale})`
	}

	$: branchTransform = (b) => {
		if (el) {
			let l = el.getTotalLength()
			let p = el.getPointAtLength(b.bp * l)
			let x = p.x
			let y = p.y
			let transform = `translate(${x}, ${y}) rotate(${b.rot}) scale(${b.scaleFac})`
			//console.log('transform: ', transform)
			return transform
		}
	}

	$: markerPoint = (el) => {
		if (el) {
			let l = el.getTotalLength()
			let endPt = el.getPointAtLength(l)
			return
		}
	}
	// LOG
	// $: console.log('bp: ', bp)
	// $: console.log('props: ', $$props)
</script>

<g transform={`translate(0,0)`}>
	<!-- <text class="branch">Branch number {branches}</text> -->
	<!-- <path bind:this={el} d={`M 0 0 Q 100 200 0 400} stroke="black" fill="transparent" /> -->
	<path bind:this={el} {d} stroke="brown" fill="transparent" stroke-width={6} />
	<!-- <circle r={4} fill="brown" /> -->
	{#if markers && branchNum === 0}
		<g transform={markerTransform}>
			<!-- <circle r={markerSize} fill="none" stroke="green" stroke-width="1" /> -->
			<ellipse
				transform={`translate(0, ${markerSize * markerY})`}
				rx={markerSize}
				ry={markerSize * markerY}
				fill="green"
				stroke="green"
				stroke-width="1"
			/>
		</g>
	{/if}
	{#if branchNum > 0}
		{#each branches as b}
			<g class="branch" transform={branchTransform(b)}>
				<svelte:self {...b} {branches} {markers} {markerSize} {markerY} branchNum={branchNum - 1} />
			</g>
		{/each}
	{/if}
</g>
