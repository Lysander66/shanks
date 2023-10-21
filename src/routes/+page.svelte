<script>
	import '@picocss/pico/css/pico.min.css'
	import * as config from '$lib/config'
	import Calendar from '$lib/components/Calendar.svelte'
</script>

<div class="sticky-footer">
	<header>
		<nav>
			<a href="/" class="title">
				<b>{config.title}</b>
			</a>
			<ul class="links">
				<li><a href="v1">Layout</a></li>
				<li><a href="/player" target="_blank">Player</a></li>
			</ul>
		</nav>
	</header>

	<main class="container">
		<Calendar />
	</main>

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
