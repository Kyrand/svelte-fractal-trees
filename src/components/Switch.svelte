<script>
	export let label = 'placeholder'
	export let flag = true
	export let id = 'switch'
</script>

<label for={id}>
	<input
		type="checkbox"
		{id}
		name={id}
		role="switch"
		checked={flag}
		on:click={() => (flag = !flag)}
	/>
	{label}
</label>
