<script>
	import LiveStream from '$lib/components/LiveStream.svelte'

	let nodes = []

	let define = {
		schemas: [
			{ id: 'rtmp', text: 'RTMP' },
			{ id: 'rtsp', text: 'RTSP' },
			{ id: 'hls', text: 'HLS' },
			{ id: 'ts', text: 'TS' },
			{ id: 'fmp4', text: 'FMP4' }
		]
	}

	let schema = define.schemas[0]
	let app = ''

	async function fetchData() {}

	function handleSubmit() {
		fetchData()
	}

	function handleChange() {
		fetchData()
	}
</script>

<div class="container">
	<!-- Form elements-->
	<!-- <section id="form">
		<form on:submit|preventDefault={handleSubmit}>
			<div class="grid">
				<fieldset>
					<label for="sel_schema">Schema</label>
					<select
						bind:value={schema}
						on:change={() => handleChange()}
						id="sel_schema"
						name="select2"
						required
					>
						{#each define.schemas as v}
							<option value={v}> {v.text} </option>
						{/each}
					</select>
				</fieldset>

				<fieldset>
					<label for="text">App</label>
					<input bind:value={app} type="text" id="text" name="text" placeholder="App" />
				</fieldset>

				<fieldset>
					<label for="submit"> &nbsp; </label>
					<input id="submit" type="submit" value="Submit" />
				</fieldset>
			</div>
		</form>
	</section> -->
</div>

<div class="grid-container">
	{#each nodes as node}
		<div class="container">
			<LiveStream nodeName={node.text} nodeIp={node.id} schema={schema.id} {app} />
		</div>
	{/each}
</div>

<style>
	.grid-container {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		gap: 2rem;
	}
</style>
