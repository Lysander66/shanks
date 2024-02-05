<script>
	import LiveStream from '$lib/components/LiveStream.svelte'

	let nodes = [
		{ id: '14.22.81.72', text: '华南2' },
		{ id: '148.113.15.73', text: '孟买' },
		{ id: '156.240.111.221', text: '香港2' },
		{ id: '156.240.111.75', text: '香港3' },
		{ id: '113.250.189.183', text: '西南1-A' },
		{ id: '113.250.189.179', text: '西南1-B' },
		{ id: '113.250.189.197', text: '西南1-C' },
		{ id: '113.250.189.196', text: '西南1-D' },
		{ id: '125.94.148.106', text: '佛山3-A' },
		{ id: '125.94.108.255', text: '佛山3-B' },
		{ id: '125.94.111.54', text: '佛山3-C' },
		{ id: '36.41.73.9', text: '西安4-A' },
		{ id: '36.41.72.43', text: '西安4-B' },
		{ id: '36.41.73.106', text: '西安4-C' },
		{ id: '36.41.73.186', text: '西安4-D' }
	]

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
