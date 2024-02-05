<script>
	import { formatTimeOnly } from '$lib/time'
	import { onMount } from 'svelte'
	import { error } from '@sveltejs/kit'

	let rawURL = ''

	let define = {
		apps: [
			{ id: '', text: 'All' },
			{ id: 'esport', text: '电竞' },
			{ id: 'sport', text: '体育' }
		]
	}

	let app = define.apps[0]
	let tableData = []

	async function fetchData() {
		const response = await fetch(`${rawURL}?app=${app.id}`)
		const resp = await response.json()
		if (resp.code != 0) {
			throw error(420, 'Enhance your calm')
		}

		tableData = resp.data.list
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

<div class="container">
	<!-- Form elements-->
	<section id="form">
		<form on:submit|preventDefault={handleSubmit}>
			<div class="grid">
				<fieldset>
					<label for="sel_app">App</label>
					<select
						bind:value={app}
						on:change={() => handleChange()}
						id="sel_app"
						name="select2"
						required
					>
						{#each define.apps as v}
							<option value={v}> {v.text} </option>
						{/each}
					</select>
				</fieldset>

				<fieldset>
					<label for="submit"> &nbsp; </label>
					<input id="submit" type="submit" value="Submit" />
				</fieldset>
			</div>
		</form>
	</section>
</div>

<!-- Tables -->
<section id="tables">
	<figure>
		<table role="grid">
			<thead>
				<tr>
					<th scope="col">#</th>
					<th scope="col">region</th>
					<th scope="col">local</th>
					<th scope="col">alive</th>
					<th scope="col">remote</th>
					<th scope="col">vcodec</th>
					<th scope="col">resolution</th>
					<th scope="col">bit rate</th>
					<th scope="col">publish time</th>
					<th scope="col">fps</th>
					<th scope="col">sample</th>
					<th scope="col">createdAt</th>
					<th scope="col">url</th>
				</tr>
			</thead>
			<tbody>
				{#if tableData && tableData.length > 0}
					{#each tableData as v, i (v.stream)}
						<tr>
							<th scope="row">{i + 1}</th>
							<td
								><a href="http://{v.ip}:8281/api/v1/stat/allGroup?schema=rtmp" target="_blank"
									>{v.region}</a
								>
							</td>
							<td
								><a
									href="http://localhost:5173/player?u=http://{v.ip}:8280/{v.app}/{v.stream}/hls.m3u8"
									target="_blank">{v.stream}</a
								>
							</td>
							<td>{Math.floor(v.alive_second / 60) + 'm ' + (v.alive_second % 60) + 's'}</td>
							<td>
								<a href="http://localhost:5173/player?u={v.link}" target="_blank"
									>{v.link ? 'play' : ''}</a
								>
							</td>
							<td>{v.vcodec}</td>
							<td>{`${v.width} x ${v.height}`}</td>
							<td>{((v.bytes_speed * 8) / 1000 / 1000).toFixed(2)} Mbps</td>
							<td>{v.publish_time}</td>
							<td>{v.fps}</td>
							<td>{`${v.sample_rate} x ${v.sample_bit}`}</td>
							<td>{formatTimeOnly(v.create_stamp)}</td>
							<td
								><span
									class={v.origin_url.startsWith('rtmp') ? 'copy-link2' : 'copy-link'}
									role="menuitem"
									tabindex="0"
									on:click={copyToClipboard}
									on:keydown={copyToClipboard}
									data-url={v.origin_url.replace(/\?\?referer=.*?&&/, '')}
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
		color: var(--primary-hover);
	}
	.copy-link2 {
		cursor: pointer;
		text-decoration: underline;
	}
</style>
