<script lang="ts">
	import Card from './Card.svelte';
</script>

<div class="home-container">
	<Card />
</div>

<style>
	.home-container {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}

	@media (max-width: 768px) and (hover: none) and (pointer: coarse) {
		.home-container {
			justify-content: flex-start;
			padding-top: 4rem;
		}
	}
</style>
