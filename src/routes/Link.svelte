<script lang="ts">
	const { name, link, onClick } = $props();
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
		color: var(--link-color);
		background-color: var(--bg-color);
		text-decoration: none;
	}

	a:hover {
		text-decoration: None;
		color: var(--bg-color);
		background-color: var(--text-color);
	}
</style>
