<script lang="ts">
	import Footer from '$lib/components/Footer.svelte';
	let text = $state('');
	let charCount = $derived(text.length);
</script>

<div class="wrapper">
	<div class="text-area-container">
		<textarea bind:value={text}></textarea>
		<p><span class="character-count">Character count:</span> {charCount}</p>
	</div>
</div>

<Footer previous="04" next="06" />

<style>
	.wrapper {
		width: 100%;
		height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.text-area-container {
		display: grid;
		width: 50%;
	}

	.text-area-container textarea {
		width: 100%;
		height: 220px;
	}

	.text-area-container p {
		margin-top: 0.5rem;
		text-align: right;
		font-size: 0.875rem;
		font-weight: 600;
	}

	.character-count {
		font-weight: 400;
		color: var(--color-text-muted);
	}
</style>
