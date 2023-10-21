<script>
	import LiveStream from '$lib/components/LiveStream.svelte'

	let nodes = []
</script>

{#each nodes as node}
	<LiveStream nodeIp={node.id} nodeName={node.text} />
{/each}
