<script>
	import Home from './Home.svelte';
</script>

<Home />

<style>
	:global(html),
	:global(body) {
		margin: 0;
		padding: 0;
		height: 100vh;
		--bg-color: #ffffff;
		--text-color: #333333;
		--watermark-text-color: #cccccc;
		background-color: var(--bg-color);
		color: var(--text-color);
	}
</style>
