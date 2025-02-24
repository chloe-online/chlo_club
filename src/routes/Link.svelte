<script>
	import { onMount } from 'svelte';

	const { name, link, onClick } = $props();

	onMount(() => {
		console.log('Link page mounted');
	});
</script>

<div class="link-container">
	<a href={link} onclick={onClick}>{name}</a>
</div>

<style>
	.link-container {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: flex-start;
		text-align: left;
	}

	a {
		font-size: 2rem;
		color: var(--text-color);
		background-color: var(--bg-color);
		text-decoration: None;
	}

	a:hover {
		text-decoration: None;
		color: var(--bg-color);
		background-color: var(--text-color);
	}
</style>
