<script>
	import * as config from '$lib/config'
	import PageTransition from './transition.svelte'
	import { page } from '$app/stores'

	export let data
</script>

<div class="sticky-footer">
	<!-- Header -->
	<header>
		<nav>
			<!-- Title -->
			<a href="/v1" aria-current={$page.url.pathname === '/'} class="title">
				<b>{config.title}</b>
			</a>

			<!-- Navigation -->
			<ul class="links">
				<li><a href="/" aria-current={$page.url.pathname === '/'}> Home </a></li>
				<li>
					<a href="/v1/stream" aria-current={$page.url.pathname === '/v1/stream'}> Stream </a>
				</li>
				<li><a href="/v1/about" aria-current={$page.url.pathname === '/v1/about'}> About </a></li>
				<li>
					<a href="/v1/contact" aria-current={$page.url.pathname === '/v1/contact'}> Contact </a>
				</li>
				<li><a href="/rss.xml" target="_blank"> RSS </a></li>
			</ul>
		</nav>
	</header>

	<!-- Main -->
	<main class="container">
		<PageTransition url={data.url}>
			<slot />
		</PageTransition>
	</main>

	<!-- Footer -->
	<footer class="footer">
		<small>Copyright &copy; {new Date().getFullYear()} Made with ❤️ by Lysander </small>
	</footer>
</div>

<style>
	header > nav {
		padding: 0 1%;
	}

	.sticky-footer {
		display: grid;
		grid-template-rows: auto 1fr auto;
		min-height: 100vh;
		background-color: #fafafa;
	}

	.footer {
		display: flex;
		justify-content: center;
	}
</style>
