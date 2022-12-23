<script>
	export let branches = 2
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

	let d
	let el,
		x = 0,
		y = 0,
		l,
		p

	$: {
		let h = curveHeight
		let w = curveWidth
		d = `M 0 0 Q ${cx * w} ${cy * h} ${dx * w} ${dy * h}`
		if (el) {
			l = el.getTotalLength()
			p = el.getPointAtLength(bp * l)
			x = p.x
			y = p.y
		}
	}

	$: console.log('bp: ', bp)
</script>

<g transform={`translate(0,0)`}>
	<!-- <text class="branch">Branch number {branches}</text> -->
	<!-- <path bind:this={el} d={`M 0 0 Q 100 200 0 400} stroke="black" fill="transparent" /> -->
	<path bind:this={el} {d} stroke="black" fill="transparent" />
	<circle r="10" />
	<g transform={`translate(${x}, ${y}) rotate(45) scale(${scaleFac})`}>
		{#if branches > 0}
			<svelte:self branches={branches - 1} {scaleFac} {cx} {cy} {dx} {dy} {bp} />
		{/if}
	</g>
</g>
