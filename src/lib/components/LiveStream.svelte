<script>
	import { formatTimeOnly } from '$lib/time'
	import { onMount } from 'svelte'
	import { error } from '@sveltejs/kit'

	export let nodeName
	export let nodeIp
	export let schema
	export let app

	let tableData = []

	async function fetchData() {
		let rawURL = `http://${nodeIp}:8281/api/stat/allGroup?schema=${schema}&app=${app}`
		console.log(nodeName, rawURL)
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

	function copyToClipboard(event) {
		const url = event.target.getAttribute('data-url')
		navigator.clipboard
			.writeText(url)
			.then(() => {
				console.log('已复制到剪贴板:', url)
				// window.alert('Copied to clipboard')
			})
			.catch((error) => {
				console.error('复制到剪贴板时出错:', error)
			})
	}
</script>

<!-- Tables -->
<section id="tables">
	<figure>
		<table role="grid">
			<thead>
				<tr>
					<th scope="col">{nodeName}</th>
					<th scope="col">stream</th>
					<th scope="col">bit rate</th>
					<th scope="col">resolution</th>
					<th scope="col">fps</th>
					<th scope="col">sample</th>
					<th scope="col">alive</th>
					<!-- <th scope="col">createdAt</th> -->
					<th scope="col">url</th>
				</tr>
			</thead>
			<tbody>
				{#if tableData && tableData.length > 0}
					{#each tableData as v, i (v.stream)}
						<tr>
							<th scope="row">{i + 1}</th>
							<td
								><a
									href="http://localhost:5173/player?u=http://{nodeIp}:8280/{v.app}/{v.stream}/hls.m3u8"
									target="_blank">{v.stream}</a
								>
							</td>
							<td>{((v.bytesSpeed * 8) / 1000 / 1000).toFixed(2)} Mbps</td>
							<td
								>{((result) => `${result.width} x ${result.height}`)(
									v.tracks.find((track) => track.codec_id_name.startsWith('H26'))
								)}</td
							>
							<td>{v.tracks.find((track) => track.codec_id_name.startsWith('H26')).fps}</td>
							<td
								>{((result) => `${result.sample_rate} x ${result.sample_bit}`)(
									v.tracks.find((track) => !track.codec_id_name.startsWith('H26'))
								)}</td
							>
							<td>{Math.floor(v.aliveSecond / 60) + 'm ' + (v.aliveSecond % 60) + 's'}</td>
							<!-- <td>{formatTimeOnly(v.createStamp)}</td> -->
							<td
								><span
									class="copy-link"
									role="menuitem"
									tabindex="0"
									on:click={copyToClipboard}
									on:keydown={copyToClipboard}
									data-url={v.originUrl.replace(/\?\?referer=.*?&&/, '')}
								>
									copy
								</span></td
							>
						</tr>
					{/each}
				{:else}
					<p>No data</p>
				{/if}
			</tbody>
		</table>
	</figure>
</section>

<style>
	.copy-link {
		cursor: pointer;
		text-decoration: underline;
		/* color: var(--primary-hover); */
	}
</style>
