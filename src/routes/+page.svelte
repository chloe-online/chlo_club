<script lang="ts">
	import Home from './Home.svelte';
</script>

<Home />

<style>
	:global(html),
	:global(body) {
		margin: 0;
		padding: 0;
		height: 100dvh;
		min-height: 100dvh;
		overflow: hidden;
		--bg-color: #ffffff;
		--text-color: #333333;
		--link-color: #333333;
		--watermark-text-color: #bbbbbb;
		background-color: var(--bg-color);
		color: var(--text-color);
	}
</style>
