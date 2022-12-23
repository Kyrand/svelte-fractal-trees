<script>
	import CircularBarChart from './components/CircularBarChart/CircularBarChart.svelte'
	import _data from './data/country_data.json'

	let data = Object.values(_data)
	let options = ['gini', 'area', 'population']
	let fieldKey = 'population'
	let sortData = true
	let showLegend = false
	let hubFactor = 0
	let hubFactorRange = 35
	let labelPadding = 0
	let labelPaddingRange = 0
	let randomThreshold = 1
	let labels = true

	$: hubFactor = hubFactorRange / 100
	$: labelPadding = labelPaddingRange / 100

	let cdata = []
	let fdata = []
	$: {
		fdata = randomizeData(fieldKey)
		cdata = [...fdata]
	}
	$: {
		if (sortData) {
			console.log('Sorting the data...')
			cdata = cdata.sort((a, b) => +b.value - +a.value)
		} else {
			cdata = [...fdata]
		}
	}

	$: console.log(
		'Country data: ',
		//cdata.map((d) => ({ name: d.name, population: d.population }))
		cdata.map((d) => ({ name: d.name, value: d.value }))
	)

	$: console.log('Field key: ', fieldKey)
	$: text = {
		title: 'Country Data',
		subtitle: `Comparing ${fieldKey}`,
		body: '<p>This dataset can be found at etc.</br>It provides comparison by area, gini-coefficient and population. '
	}

	function randomizeData(fieldKey) {
		return data
			.filter((d) => Math.random() < randomThreshold && numberCheck(d[fieldKey]))
			.map((d) => {
				return { name: d.name, value: d[fieldKey] }
			})
	}

	function numberCheck(x) {
		return x !== null && !Number.isNaN(x)
	}
</script>

<div class="container">
	<h1>Circular bar chart with Svelte and Layercake</h1>
	<p>
		This demo uses a small country dataset containing population size, area and <a
			href="https://en.wikipedia.org/wiki/Gini_coefficient">gini coefficient.</a
		> The latter gives a statistical measure of income inequality, the larger the number the more unequal
		the society.
	</p>
	<div class="demo__wrapper">
		<div class="ui">
			<label for="field">Field</label>
			<select bind:value={fieldKey} id="field" required>
				{#each options as opt}
					<option value={opt} selected={fieldKey === opt}>{opt}</option>
				{/each}
			</select>
			<!-- <button
			on:click={() => {
				cdata = randomizeData(fieldKey)
			}}
			class="_pure-button _pure-button-primary">Randomize</button
		> -->
			<label for="switch">
				<input
					type="checkbox"
					id="switch"
					name="switch"
					role="switch"
					checked={sortData}
					on:click={() => (sortData = !sortData)}
				/>
				Sort the data
			</label>
			<label for="switch">
				<input
					type="checkbox"
					id="switch"
					name="switch"
					role="switch"
					checked={showLegend}
					on:click={() => (showLegend = !showLegend)}
				/>
				Show legend
			</label>
			<label
				>Hub radius
				<input bind:value={hubFactorRange} type="range" min="0" max="100" />
			</label>
			<label
				>Label padding
				<input bind:value={labelPaddingRange} type="range" min="0" max="100" />
			</label>
		</div>

		<div class="cbc">
			<CircularBarChart
				data={cdata}
				{hubFactor}
				{labelPadding}
				{text}
				legend={showLegend}
				labels={!showLegend}
			/>
		</div>
	</div>
</div>

<style>
	.demo__wrapper {
		display: flex;
		flex-direction: row;
		padding: 1em;
	}

	.ui {
		display: flex;
		flex-direction: column;
		/* gap: 1em; */
	}

	.cbc {
		width: 800px;
		height: 800px;
	}
</style>
