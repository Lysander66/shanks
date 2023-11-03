<script>
	import { formatTime } from '$lib/time'
	import { onMount } from 'svelte'
	import { error } from '@sveltejs/kit'

	export let nodeIp
	export let nodeName

	let define = {
		schemas: [
			{ id: 'rtmp', text: 'RTMP' },
			{ id: 'rtsp', text: 'RTSP' },
			{ id: 'hls', text: 'HLS' },
			{ id: 'ts', text: 'TS' },
			{ id: 'fmp4', text: 'FMP4' }
		]
	}

	let schema
	let app = ''

	let tableData = []

	async function fetchData() {
		let rawURL = `http://${nodeIp}:8281/api/stat/allGroup?schema=${schema.id}&app=${app}`
		console.log('raw', rawURL)
		const response = await fetch(rawURL)
		const resp = await response.json()
		if (resp.code != 0) {
			throw error(420, 'Enhance your calm')
		}

		tableData = resp.data
	}

	onMount(() => {
		fetchData()
	})

	function handleSubmit() {
		fetchData()
	}

	function handleChange() {
		fetchData()
	}
</script>

<!-- Form elements-->
<section id="form">
	<form on:submit|preventDefault={handleSubmit}>
		<h2>{nodeName}</h2>

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
</section>

<!-- Tables -->
<section id="tables">
	<!-- <h2>Tables</h2> -->
	<figure>
		<table role="grid">
			<thead>
				<tr>
					<th scope="col">#</th>
					<th scope="col">url</th>
					<th scope="col">app</th>
					<th scope="col">stream</th>
					<th scope="col">bit rate (kbit/s)</th>
					<th scope="col">fps</th>
					<th scope="col">resolution</th>
					<th scope="col">sample</th>
					<th scope="col">alive</th>
					<th scope="col">createdAt</th>
					<!-- <th scope="col">originUrl</th> -->
				</tr>
			</thead>
			<tbody>
				{#if tableData && tableData.length > 0}
					{#each tableData as v, i (v.stream)}
						<tr>
							<th scope="row">{i + 1}</th>
							<td><a href="http://{nodeIp}:8280/{v.app}/{v.stream}/hls.m3u8">play</a></td>
							<!-- <td><a href="http://{nodeIp}:8280/{v.app}/{v.stream}.live.flv">play</a></td> -->
							<td>{v.app}</td>
							<td>{v.stream}</td>
							<td>{Math.floor((v.bytesSpeed * 8) / 1000)}</td>
							<td>{v.tracks.find((track) => track.codec_id_name.startsWith('H26')).fps}</td>
							<td
								>{((result) => `${result.width} x ${result.height} ${result.codec_id_name}`)(
									v.tracks.find((track) => track.codec_id_name.startsWith('H26'))
								)}</td
							>
							<td
								>{((result) => `${result.sample_rate} x ${result.sample_bit}`)(
									v.tracks.find((track) => !track.codec_id_name.startsWith('H26'))
								)}</td
							>
							<td>{Math.floor(v.aliveSecond / 60) + 'm ' + (v.aliveSecond % 60) + 's'}</td>
							<td>{formatTime(v.createStamp)}</td>
							<!-- <td>{v.originUrl}</td> -->
						</tr>
					{/each}
				{:else}
					<p>No data</p>
				{/if}
			</tbody>
		</table>
	</figure>
</section>
