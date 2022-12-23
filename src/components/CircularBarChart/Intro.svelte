<script>
	import { getContext } from 'svelte'

	export let text
	export let hubFactor

	const { config, width, height, data, xGet, yGet, xScale, yScale, extents } =
		getContext('LayerCake')
</script>

<div class="intro__wrapper">
	<div class="intro" style={`width: ${0.8 * hubFactor * 100}%`}>
		<h2>{text.title}</h2>
		{#if text.subtitle}
			<h3>{text.subtitle}</h3>
		{/if}
		{#if text.body}
			<p>
				{@html text.body}
			</p>
		{/if}
	</div>
</div>

<style>
	.intro__wrapper {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 100%;
	}

	.intro {
		/* flex: 1; */
		--line-height: 1.1;
	}

	h2 {
		margin: 0;
		--font-size: 1.2rem;
	}
	h3 {
		margin: 0;
		--font-size: 1rem;
	}
	.intro p {
		line-height: 0.9;
		--font-size: 0.8rem;
	}
</style>
