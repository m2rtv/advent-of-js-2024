<script lang="ts">
	import Footer from '$lib/components/Footer.svelte';
	let text = $state('');
	let title = $derived.by(() => {
		let title = '/';
		title += text.toLowerCase().replace(/[^a-z0-9]/g, '-');
		return title;
	});
</script>

<div class="wrapper">
	<div class="text-area-container">
		<!-- <label for="text">Title</label> -->
		<input id="text" placeholder="Add title to slugify..." bind:value={text} />
		<span class="slugified-title">{title}</span>
	</div>
</div>

<Footer previous="06" next="08" />

<style>
	.wrapper {
		width: 100%;
		height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 0 1rem;
		background-color: var(--brand-color-100);
	}

	.text-area-container {
		display: grid;
		width: min(100%, 360px);
	}

	.slugified-title {
		margin-top: 0.5rem;
		padding-left: 1rem;
		font-weight: 400;
		color: rgba(0, 0, 0, 0.5);
		font-size: 0.75rem;
	}
</style>
