<script>
	import { onMount, onDestroy, afterUpdate } from 'svelte'
	import Hls from 'hls.js'

	export let sourceUrl

	let videoElement
	let hls

	onMount(() => {
		initHlsPlayer()
	})

	afterUpdate(() => {
		if (sourceUrl) {
			initHlsPlayer()
		}
	})

	onDestroy(() => {
		if (hls) {
			hls.destroy()
		}
	})

	function initHlsPlayer() {
		// First check for native browser HLS support
		if (videoElement.canPlayType('application/vnd.apple.mpegurl')) {
			videoElement.src = sourceUrl
			videoElement.addEventListener('canplay', () => {
				videoElement.play()
			})
			console.log('native HLS')
			return
		}

		// If no native HLS support, check if HLS.js is supported
		if (Hls.isSupported()) {
			hls = new Hls()
			hls.loadSource(sourceUrl)
			hls.attachMedia(videoElement)
			hls.on(Hls.Events.MANIFEST_PARSED, () => {
				videoElement.play()
			})
			console.log('HLS.js')
			return
		}
	}
</script>

<video bind:this={videoElement} controls autoplay>
	<track default kind="captions" />
</video>
