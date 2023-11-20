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
</script>

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
