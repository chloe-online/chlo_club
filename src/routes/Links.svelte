<script lang="ts">
	import Link from './Link.svelte';

	function handleClick(e: MouseEvent) {
		e.stopPropagation();
	}
</script>

<div class="links-container">
	<h2>things chloe does</h2>
	<ul class="links-list">
		<li>
			<Link name="shot diary" link="https://chlo.exposed" onClick={handleClick} />
		</li>
		<li>
			<Link name="thoughts" link="https://www.tumblr.com/blog/nowhere2chlo" onClick={handleClick} />
		</li>
		<li>
			<Link name="music" link="https://soundcloud.com/kloe_e_club" onClick={handleClick} />
		</li>
		<li>
			<Link name="games" link="https://chloclub.itch.io" onClick={handleClick} />
		</li>
	</ul>
	<hr />
	<ul class="socials-list">
		<li>
			<a href="https://www.instagram.com/nowhere2chlo" onclick={handleClick}>ig</a>
		</li>
		<li>
			<a href="https://www.twitter.com/chlo_exposed" onclick={handleClick}>x</a>
		</li>
		<li>
			<a href="https://www.pi.fyi/u/chloexposed" onclick={handleClick}>pi</a>
		</li>
		<li>
			<a href="https://bsky.app/profile/chloexposed.bsky.social" onclick={handleClick}>bsky</a>
		</li>
	</ul>
</div>

<style>
	.links-container {
		display: flex;
		flex-direction: column;
		align-items: left;
		justify-content: left;
		gap: 1rem;
	}

	h2 {
		margin: 0;
		font-size: 2rem;
		font-style: normal;
		font-weight: 400;
		font-family: inherit;
		color: var(--watermark-text-color);
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

	.socials-list {
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		text-align: center;
		margin: 0px;
		gap: 1rem;
		padding: 0;
		font-size: 1rem;
	}

	.socials-list li {
		list-style: none;
		position: relative;
	}

	.socials-list li::before {
		content: '[';
		margin-right: 0.1em;
		color: var(--watermark-text-color);
	}

	.socials-list li::after {
		content: ']';
		margin-left: 0.1em;
		color: var(--watermark-text-color);
	}

	.socials-list a {
		text-decoration: none;
		color: var(--text-color);
		padding: 0 0.2rem 0 0.2rem;
	}

	.socials-list a:hover {
		background-color: var(--text-color);
		color: var(--bg-color);
	}

	hr {
		border: 0;
		height: 1px;
		background-color: var(--watermark-text-color);
		margin: 1rem 0;
	}
</style>
