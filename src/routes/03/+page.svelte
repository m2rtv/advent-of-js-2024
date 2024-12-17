<script lang="ts">
	import Footer from '$lib/components/Footer.svelte';

	let textareaEl = $state<HTMLTextAreaElement | null>(null);

	function adjustHeight() {
		if (textareaEl) {
			textareaEl.style.height = 'auto';
			textareaEl.style.height = textareaEl.scrollHeight + 'px';
		}
	}
</script>

<div class="wrapper">
	<div class="text-area-container">
		<textarea bind:this={textareaEl} on:input={adjustHeight} placeholder="Start typing..."
		></textarea>
	</div>
</div>

<Footer previous="02" next="04" />

<style>
	.wrapper {
		width: 100%;
		height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: var(--brand-color-100);
	}

	textarea {
		resize: none;
		overflow-y: hidden;
		padding-bottom: 0px;
	}

	.text-area-container {
		width: 50%;
	}
</style>
