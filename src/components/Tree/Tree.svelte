<script>
	import Branch from './Branch.svelte'
	export let branches = []
	export let branchNum = 3
	export let cx = 0.5
	export let cy = 0.5
	export let dx = 0
	export let dy = 1
	export let rot = 45
	export let curveHeight = 600
	export let curveWidth = 600
	export let markers = true

	let el, d

	$: {
		let h = curveHeight
		let w = curveWidth
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

<g transform={`translate(0,0)`}>
	<!-- <text class="branch">Branch number {branches}</text> -->
	<!-- <path bind:this={el} d={`M 0 0 Q 100 200 0 400} stroke="black" fill="transparent" /> -->
	<path bind:this={el} {d} stroke="black" fill="transparent" />

	{#each branches as b}
		<g class="branch" transform={branchTransform(b)}>
			<Branch {...b} {branchNum} {branches} {markers} />
		</g>
	{/each}
</g>
