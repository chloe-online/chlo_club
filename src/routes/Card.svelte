<script lang="ts">
	import { onMount } from 'svelte';

	import Links from './Links.svelte';

	let flipped = $state(true);
	let showHelpText = $state(false);
	let helpTextTimer = $state(10000);
	let isDragging = $state(false);
	let rotation = $state(0);
	let touchStartX = $state(0);

	const TOUCH_FLIP_THRESHOLD = 45;

	function toggleFlip() {
		flipped = !flipped;
		showHelpText = false;

		// Clear any existing timer
		if (helpTextTimer) {
			clearTimeout(helpTextTimer);
		}

		// If we're now on the back side, start the timer
		if (flipped) {
			helpTextTimer = setTimeout(() => {
				showHelpText = true;
			}, 10000);
		}
	}

	function handleTouchStart(event: TouchEvent) {
		touchStartX = event.touches[0].clientX;
		isDragging = true;
	}

	function handleTouchMove(event: TouchEvent) {
		if (!isDragging) return;

		const touchX = event.touches[0].clientX;
		const deltaX = touchX - touchStartX;

		// Convert the delta to degrees (adjust sensitivity as needed)
		rotation = (deltaX / 2) % 360;

		// If rotation exceeds threshold, update the flipped state
		if (Math.abs(rotation) > TOUCH_FLIP_THRESHOLD) {
			flipped = !flipped;
			// Don't reset rotation, let it smoothly transition
			touchStartX = touchX;
			isDragging = false;
		}
	}

	function handleTouchEnd() {
		isDragging = false;
		rotation = 0;
	}

	onMount(() => {
		// Initial timer for when the page loads
		if (flipped) {
			helpTextTimer = setTimeout(() => {
				showHelpText = true;
			}, 10000);
		}
	});
</script>

<div class="card-container">
	<div class="card" class:flipped class:sudden={!isDragging} role="button" tabindex="0">
		<div
			class="card-faces"
			class:dragging={isDragging}
			style="transform: rotateY({rotation + (flipped ? -180 : 0)}deg)"
		>
			<div class="card-front">
				<div
					class="card-inner-container"
					onclick={toggleFlip}
					onkeydown={toggleFlip}
					role="button"
					tabindex="0"
				>
					<Links />
					<div class="footer">
						<p>always made with &lt;3</p>
						<p>by chloe</p>
					</div>
				</div>
			</div>
			<div class="card-back">
				<div
					class="card-inner-container"
					onclick={toggleFlip}
					onkeydown={toggleFlip}
					role="button"
					tabindex="0"
				>
					<h2>♣</h2>
					<div class="card-pattern">
						<h1>JOIN THE CLUB</h1>
					</div>
					<h2 style="transform: rotate(180deg)">♣</h2>
				</div>
			</div>
		</div>
	</div>
	<div class="help-text" class:visible={showHelpText}>
		<p>⟳ flip me...</p>
	</div>
</div>

<style>
	/* Card Base Styles */
	.card {
		perspective: 1000px;
		width: min(320px, 90vw);
		aspect-ratio: 1 / 1.4;
		margin: auto;
		cursor: pointer;
	}

	.card-faces {
		position: relative;
		width: 100%;
		height: 100%;
		transform-style: preserve-3d;
		transition: transform 0.4s ease-out;
	}

	/* Add sudden transition for non-touch flips */
	.card.sudden .card-faces {
		transition: transform 0.8s;
	}

	.card-faces.dragging {
		transition: none;
	}

	/* Card Container */
	.card-container {
		position: relative;
		width: fit-content;
		margin: 0;
	}

	/* Card Faces (Front & Back) */
	.card-front,
	.card-back {
		position: absolute;
		width: 100%;
		height: 100%;
		backface-visibility: hidden;
		-webkit-backface-visibility: hidden; /* Safari support */
	}

	.card-front {
		transform: rotateY(0deg);
	}

	.card-back {
		transform: rotateY(180deg);
		background-color: var(--background-color);
	}

	/* Card Content Styles */
	.card-pattern {
		flex: 1;
		background-size: 10px 10px;
		display: flex;
		justify-content: center;
		align-items: center;
		text-align: center;
		user-select: none;
		-webkit-user-select: none; /* Safari support */
	}

	.card-pattern h1,
	.card-back h1,
	.card-back h2 {
		margin: 0;
		font-size: 4rem;
		font-style: normal;
		font-weight: 400;
		font-family: inherit;
		line-height: 1;
	}

	.card-back h2 {
		padding: 0;
		line-height: 0.7;
		height: fit-content;
		color: var(--text-color);
		user-select: none;
		-webkit-user-select: none; /* Safari support */
	}

	/* Card Container */
	.card-inner-container {
		height: 100%;
		margin: 0;
		display: flex;
		flex-direction: column;
		align-items: left;
		justify-content: left;
		text-align: left;
		color: var(--text-color);
		font-size: 1.5rem;
		border: 1px solid var(--text-color);
		border-radius: 30px;
		padding: 30px;
		width: min(320px, 90vw);
		aspect-ratio: 1 / 1.4;
		box-sizing: border-box;
		margin: auto;
	}

	/* Footer Styles */
	.footer {
		margin: 0;
		margin-top: auto;
		display: flex;
		flex-direction: column;
		align-items: flex-end;
		text-align: right;
		font-size: 1rem;
		font-style: italic;
		color: var(--watermark-text-color);
	}

	.footer p {
		margin: 0;
		line-height: 1.2;
	}

	/* Help Text */
	.help-text {
		position: absolute;
		width: 100%;
		text-align: left;
		bottom: -2rem;
		left: -0.5rem;
		font-size: 0.9rem;
		color: var(--watermark-text-color);
		opacity: 0;
		transition: opacity 0.5s ease-in-out;
	}

	.help-text.visible {
		opacity: 1;
	}

	.help-text p {
		margin: 0;
	}
</style>
