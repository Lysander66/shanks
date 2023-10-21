<script>
	import * as config from '$lib/config'
	import HLSPlayer from '$lib/components/HlsPlayer.svelte'
	import { onMount } from 'svelte'
	import { page } from '$app/stores'

	let streamUrl = ''
	let sourceUrl = ''

	function onClick() {
		sourceUrl = streamUrl
	}

	onMount(() => {
		// https://kit.svelte.dev/docs/load#using-url-data-url
		for (const [key, value] of $page.url.searchParams) {
			// console.log(`URLSearchParams: ${key}: ${value}`)
		}
		console.log('url', $page.url.searchParams.get('u'))
	})
</script>

<div class="sticky-footer">
	<header>
		<nav>
			<a href="/" class="title">
				<b>{config.title}</b>
			</a>
		</nav>
	</header>

	<main class="container">
		<div class="form">
			<fieldset>
				<label for="input">Stream URL:</label>
				<input type="text" bind:value={streamUrl} id="streamUrl" />
			</fieldset>

			<fieldset>
				<label for="submit"> &nbsp; </label>
				<button on:click={onClick}>Play</button>
			</fieldset>
		</div>

		<div class="video">
			<HLSPlayer sourceUrl={$page.url.searchParams.get('u')} />
		</div>
	</main>
</div>

<style>
	header > nav {
		padding: 0 1%;
	}

	.sticky-footer {
		display: grid;
		grid-template-rows: auto 1fr auto;
		min-height: 100vh;
		background-image: url('./deepsea.webp');
		background-size: cover;
		background-position: center;
	}

	.video {
		display: flex;
		justify-content: center;
		margin: 0 0 5rem 0;
	}

	.form {
		display: flex;
		visibility: hidden;
	}

	.form fieldset:first-child {
		flex: 1;
	}

	.form fieldset:last-child {
		flex-basis: 7rem;
		margin-left: 2rem;
	}
</style>
