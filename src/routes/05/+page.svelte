<script lang="ts">
	import Footer from '$lib/components/Footer.svelte';
	let text = $state('');
	let charCount = $derived(text.length);
</script>

<div class="wrapper">
	<div class="text-area-container">
		<textarea placeholder="Start typing..." bind:value={text}></textarea>
		<p><span class="character-count">Character count:</span> {charCount}</p>
	</div>
</div>

<Footer previous="04" next="06" />

<style>
	.wrapper {
		width: 100%;
		height: 100vh;
		padding: 0 1rem;
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: var(--brand-color-100);
	}

	.text-area-container {
		display: grid;
		width: min(100%, var(--width-md));
	}

	.text-area-container textarea {
		height: 160px;
	}

	.text-area-container p {
		text-align: right;
		margin-top: 0.5rem;
		font-weight: 600;
	}

	.character-count {
		font-weight: 400;
		color: rgba(0, 0, 0, 0.5);
	}
</style>
