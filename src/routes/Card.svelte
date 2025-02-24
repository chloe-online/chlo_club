<script>
	import { onMount } from 'svelte';

	import Link from './Link.svelte';

	let flipped = $state(true);
	let showHelpText = $state(false);
	let helpTextTimer = $state(10000);

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
		onclick={(e) => {
			toggleFlip();
		}}
		onkeydown={(e) => e.key === 'Enter' && toggleFlip()}
		role="button"
		tabindex="0"
	>
		<div class="card-inner" class:flipped>
			<div class="card-front">
				<div class="home-container">
					<h2>where to find chloe</h2>
					<ul class="links-list">
						<li>
							<Link name="shot diary" link="https://chlo.exposed" />
						</li>
						<li>
							<Link name="thoughts" link="https://www.tumblr.com/blog/nowhere2chlo" />
						</li>
						<li>
							<Link name="music" link="https://soundcloud.com/kloe_e_club" />
						</li>
						<li>
							<Link name="games" link="https://chloe-online.itch.io" />
						</li>
					</ul>
					<div class="footer">
						<p>always made with &lt;3</p>
						<p>by chloe</p>
					</div>
				</div>
			</div>
			<div class="card-back">
				<div class="home-container">
					<h1>♣</h1>
					<!-- <h1>chlo.club</h1> -->
					<div class="card-pattern"></div>
					<h1 style="transform: rotate(180deg)">♣</h1>
				</div>
			</div>
		</div>
	</div>
	<div class="help-text" class:visible={showHelpText}>
		<p>flip me...</p>
	</div>
</div>

<style>
	.card {
		perspective: 1000px;
		width: min(320px, 90vw);
		aspect-ratio: 1 / 1.4;
		margin: auto;
		cursor: pointer;
	}

	.card-inner {
		position: relative;
		width: 100%;
		height: 100%;
		transition: transform 0.8s;
		transform-style: preserve-3d;
	}

	.card-inner.flipped {
		transform: rotateY(-180deg);
	}

	.card-front,
	.card-back {
		position: absolute;
		width: 100%;
		height: 100%;
		backface-visibility: hidden;
		-webkit-backface-visibility: hidden; /* For Safari support */
	}

	.card-front {
		transform: rotateY(0deg);
	}

	.card-back {
		transform: rotateY(180deg);
		background-color: var(--background-color);
	}

	.card-pattern {
		flex: 1;
		background-size: 10px 10px;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.home-container {
		height: 100%;
		margin: 0;
	}

	.home-container {
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
		/* box-shadow: 0 0 10px 0 rgba(0, 0, 0, 0.3); */
	}

	.home-container h1 {
		margin: 0px;
		font-size: 4rem;
		font-style: normal;
		font-weight: 400;
		font-family: inherit;
		line-height: 1;
	}

	.home-container h2 {
		margin: 0 0 20px 0;
		font-size: 2rem;
		font-style: normal;
		font-weight: 400;
		font-family: inherit;
	}

	.links-list {
		display: flex;
		flex-direction: column;
		align-items: left;
		justify-content: left;
		margin: 0px;
		padding-left: 1rem;
	}

	.links-list li {
		list-style: none;
	}

	.footer {
		margin: 0px;
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

	.card-container {
		position: relative;
		width: fit-content;
		margin: 0;
	}

	.help-text {
		position: absolute;
		width: 100%;
		text-align: left;
		bottom: -2rem;
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
