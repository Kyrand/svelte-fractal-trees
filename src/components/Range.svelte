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
		<input bind:value type="range" {min} {max} />
		<div class="value">{value}</div>
	</div>
</div>

<style>
	.range__wrapper {
		display: flex;
		align-items: center;
		gap: 1em;
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
