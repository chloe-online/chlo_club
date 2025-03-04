<script lang="ts">
	import { onMount } from 'svelte';

	import Links from './Links.svelte';

	let flipped = $state(true);
	let showHelpText = $state(false);
	let helpTextTimer = $state(10000);
	let isDragging = $state(false);
	let isFlipping = $state(false);
	let rotation = $state(180);
	let verticalRotation = $state(0);
	let restingRotation = $state(180);
	let touchStartX = $state(0);
	let touchStartY = $state(0);
	let touchStartRotation = $state(0);
	let horizontalRotation = $state(0);
	let verticalTouchRotation = $state(0);

	const TOUCH_FLIP_THRESHOLD = 30;
	const TOUCH_SENSITIVITY = 0.5;
	const VERTICAL_SENSITIVITY = 0.5;
	const VERTICAL_CLAMP_DEGREES = 5;
	const EASY_THRESHOLD = 5;
	const RESISTANCE_FACTOR = 0.15;
	const MAX_ROTATION = 5;

	function toggleFlip() {
		flipped = !flipped;
		showHelpText = false;

		isFlipping = true;

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

		setTimeout(() => {
			isFlipping = false;
		}, 800);
	}

	function handleClick(event: MouseEvent) {
		toggleFlip();
		const nearestMultiple = Math.round(rotation / 180) * 180;
		if (event instanceof MouseEvent) {
			const cardWidth = event.currentTarget.offsetWidth;
			const offsetFromCenter = event.offsetX - cardWidth / 2;
			restingRotation = nearestMultiple + 180 * Math.sign(offsetFromCenter);
		}
		rotation = restingRotation;
	}

	function handleKeyDown(event: KeyboardEvent) {
		const nearestMultiple = Math.round(rotation / 180) * 180;
		if (event.key === ' ') {
			toggleFlip();
			restingRotation = nearestMultiple + 180;
		}

		rotation = restingRotation;
	}

	function handleTouchStart(event: TouchEvent) {
		touchStartX = event.touches[0].clientX;
		touchStartY = event.touches[0].clientY;
		touchStartRotation = restingRotation;
		isDragging = true;
	}

	function handleTouchMove(event: TouchEvent) {
		if (!isDragging) return;

		const touchX = event.touches[0].clientX;
		const touchY = event.touches[0].clientY;

		// Horizontal rotation (card flip) remains the same
		const deltaX = touchX - touchStartX;
		rotation = deltaX * TOUCH_SENSITIVITY + restingRotation;

		// Calculate vertical tilt with smooth resistance
		const deltaY = touchY - touchStartY;
		const rawVerticalRotation = deltaY * TOUCH_SENSITIVITY;

		//Improved resistance formula
		if (Math.abs(rawVerticalRotation) > EASY_THRESHOLD) {
			const excess = Math.abs(rawVerticalRotation) - EASY_THRESHOLD;
			// Use arctangent for a smooth curve that approaches the max value
			const resistedExcess =
				(Math.atan(excess * RESISTANCE_FACTOR) / (Math.PI / 2)) * (MAX_ROTATION - EASY_THRESHOLD);
			verticalTouchRotation = Math.sign(rawVerticalRotation) * (EASY_THRESHOLD + resistedExcess);
		} else {
			verticalTouchRotation = rawVerticalRotation;
		}

		// Final safety clamp
		verticalTouchRotation = Math.max(Math.min(verticalTouchRotation, MAX_ROTATION), -MAX_ROTATION);
	}

	function handleTouchEnd(event: TouchEvent) {
		isDragging = false;

		// Calculate the total rotation from where we started
		const totalRotation = rotation - touchStartRotation;

		// If we've rotated past the threshold, flip the card
		if (Math.abs(totalRotation) > TOUCH_FLIP_THRESHOLD) {
			restingRotation = touchStartRotation + 180 * Math.sign(totalRotation);
			toggleFlip();
		} else {
			// If we haven't rotated enough, snap back to starting position
			restingRotation = touchStartRotation;
		}

		rotation = restingRotation;
		verticalTouchRotation = 0; // Reset vertical rotation
	}

	function handleMouseMove(event: MouseEvent) {
		followPointer(event);
	}

	function followPointer(event: MouseEvent | TouchEvent) {
		if (isFlipping) return;

		const windowHeight = window.innerHeight;
		const windowWidth = window.innerWidth;

		// Calculate vertical rotation
		const verticalOffsetFromCenter = event.clientY - windowHeight / 2;
		verticalRotation = -verticalOffsetFromCenter * VERTICAL_SENSITIVITY;

		// Calculate horizontal rotation (similar to vertical)
		const horizontalOffsetFromCenter = event.clientX - windowWidth / 2;
		horizontalRotation = horizontalOffsetFromCenter * VERTICAL_SENSITIVITY; // Negative for natural feeling movement

		// Clamp both rotations
		verticalRotation = Math.max(
			Math.min(verticalRotation, VERTICAL_CLAMP_DEGREES),
			-VERTICAL_CLAMP_DEGREES
		);
		horizontalRotation = Math.max(
			Math.min(horizontalRotation, VERTICAL_CLAMP_DEGREES),
			-VERTICAL_CLAMP_DEGREES
		);
	}

	function handleMouseLeave() {
		verticalRotation = 0;
		horizontalRotation = 0;
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
	<div
		class="card"
		class:flipped
		class:sudden={!isDragging}
		role="button"
		tabindex="0"
		ontouchstart={handleTouchStart}
		ontouchmove={handleTouchMove}
		ontouchend={handleTouchEnd}
		onmousemove={handleMouseMove}
		onmouseleave={handleMouseLeave}
	>
		<div
			class="card-faces"
			class:dragging={isDragging}
			style="transform: rotateY({isDragging
				? rotation
				: rotation + horizontalRotation}deg) rotateX({isDragging
				? verticalTouchRotation
				: verticalRotation}deg)"
		>
			<div class="card-front">
				<div
					class="card-inner-container"
					onclick={handleClick}
					onkeydown={handleKeyDown}
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
					onclick={handleClick}
					onkeydown={handleKeyDown}
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
		color: var(--watermark-text-color);
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
		outline: 1px solid transparent;
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
