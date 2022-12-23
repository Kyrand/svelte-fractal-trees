<script>
	import { onMount } from 'svelte'
	export let value = 0.5
	export let min = 0
	export let max = 1
	export let rmin = 0
	export let rmax = 100
	export let label = null
	let set = false
	let range = max - min
	let rvalue = rmax * (range * (value - min))

	$: {
		console.log(range, rvalue, rmax)
		value = min + range * (rvalue / rmax)
	}

	// onMount(() => {
	// 	console.log('Mounting range...')
	// 	rvalue = rmax * (range * (value - min))
	// })
</script>

<div class="_range__wrapper">
	{#if label}
		<label>{label}</label>
	{/if}
	<div class="range__wrapper">
		<!-- <input bind:value={rvalue} type="range" {rmin} {rmax} /> -->
		<div class="range__slider">
			<input bind:value={rvalue} type="range" min={rmin} max={rmax} />
		</div>
		<div class="range__value">{value}</div>
	</div>
</div>

<style>
	.range__wrapper {
		grid-template-columns: 4fr 1fr;
		align-items: center;
		gap: 1em;
	}

	.range__slider {
		grid-column: 1;
	}

	.range__value {
		grid-column: 2;
	}

	input {
		margin-bottom: 0;
	}

	.input,
	._value {
		display: inline-block;
		vertical-align: middle;
	}
</style>
