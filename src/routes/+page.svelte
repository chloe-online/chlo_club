<script>
	import Home from './Card.svelte';
</script>

<div class="page-container">
	<Home />
</div>

<style>
	:global(html),
	:global(body) {
		margin: 0;
		padding: 0;
		height: 100vh;
		--bg-color: #ffffff;
		--text-color: #333333;
		--watermark-text-color: #dddddd;
		background-color: var(--bg-color);
		color: var(--text-color);
	}

	.page-container {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}
</style>
