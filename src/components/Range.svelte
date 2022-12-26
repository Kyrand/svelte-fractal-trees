<script>
	export let value01 = 0.5
	export let value = 50
	export let min = 0
	export let max = 100
	export let label = null
	let set = false

	$: value01 = value / max
</script>

<div class="_range__wrapper">
	{#if label}
		<label>{label}</label>
	{/if}
	<div class="range__wrapper">
		<div class="range__slider">
			<input bind:value type="range" {min} {max} />
		</div>
		<div class="range__value">{value}</div>
	</div>
</div>

<style>
	.range__wrapper {
		display: grid;
		grid-template-columns: 3fr 1fr;
		align-items: center;
		gap: 1em;
	}

	.range__slider {
		grid-column: 1;
	}

	.range__value {
		grid-column: 2;
		width: 80px;
	}

	input {
		margin-bottom: 0;
	}
</style>
