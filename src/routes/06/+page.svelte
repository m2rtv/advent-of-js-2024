<script lang="ts">
	import Icon from '$lib/components/ui/Icon/Icon.svelte';
	import Footer from '$lib/components/Footer.svelte';

	let copiedToClipboard = $state(false);
	let text = $state('Copy input text to clipboard');
	let tooltipText = $state('Copy to clipboard');

	let copyToClipboard = () => {
		navigator.clipboard.writeText(text);
		tooltipText = 'Copied!';
		copiedToClipboard = true;
		setTimeout(() => {
			copiedToClipboard = false;
			tooltipText = 'Copy to clipboard';
		}, 3000);
	};
</script>

<div class="wrapper">
	<div class="clipboard-input-wrapper">
		<input bind:value={text} type="text" autocomplete="off" />
		<button type="button" onclick={copyToClipboard}>
			<Icon
				iconName={copiedToClipboard ? 'circle-check' : 'clipboard'}
				color={copiedToClipboard ? 'var(--brand-color)' : 'black'}
			/>
			<div
				style="background-color: {copiedToClipboard ? 'var(--brand-color)' : 'black'}"
				class="tooltip"
			>
				<span style="position: relative; z-index: 1;">{tooltipText}</span>
			</div>
		</button>
	</div>
</div>

<Footer previous="05" next="07" />

<style>
	.wrapper {
		min-height: 100vh;
		padding: 0 1rem;
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: var(--brand-color-bg);
		z-index: -1;
		overflow-x: hidden;
	}

	.clipboard-input-wrapper {
		width: min(100%, 320px);
		display: flex;
		align-items: center;
		background-color: #fff;
		padding-right: 1rem;
		border-radius: var(--radius);
		border: 1px solid var(--brand-color-200);
	}

	.clipboard-input-wrapper input {
		width: 100%;
		border-width: 0px;
		padding-right: 0px;
	}
	.clipboard-input-wrapper button {
		position: relative;
	}

	.tooltip {
		display: none;
		position: absolute;
		top: -36px;
		left: 50%;
		transform: translateX(-50%);
		border-radius: var(--radius);
		padding: 0.25rem 0.5rem;
		width: max-content;
		font-size: 0.75rem;
		color: var(--brand-color-100);
		z-index: 2;
	}

	.tooltip::before {
		content: '';
		position: absolute;
		bottom: -4px;
		left: 50%;
		transform: translateX(-50%) rotate(45deg);
		width: 12px;
		height: 12px;
		border-radius: 2px;
		background-color: inherit;
		z-index: 1;
	}

	.clipboard-input-wrapper button:hover .tooltip {
		display: block;
	}
</style>
