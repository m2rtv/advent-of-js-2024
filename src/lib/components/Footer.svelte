<script lang="ts">
	interface Props {
		color?: string;
		next?: string;
		previous?: string;
		nextDisabled?: boolean;
		previousDisabled?: boolean;
	}

	let {
		color = '#000',
		next = '',
		previous = '',
		nextDisabled = false,
		previousDisabled = false
	}: Props = $props();
</script>

<header></header>

<footer>
	<div class="footer-menu">
		<a style="color: {color}" href="./" aria-label="Home">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				width="24"
				height="24"
				viewBox="0 0 24 24"
				fill="none"
				class="x47corl x1b1b1ih x98svd5 xnnoyiz"
				><path
					d="M8 16H16M4 9V20H20V9L12 2.5L4 9Z"
					stroke="white"
					stroke-width="2"
					stroke-linecap="square"
				></path></svg
			>
		</a>
		{#if previousDisabled}
			<span class="disabled">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					viewBox="0 0 24 24"
					fill="none"
					class="x47corl x1b1b1ih x98svd5 xnnoyiz"
					><path
						d="M7 3L4 6L7 9M5 6H13.5C17.0899 6 20 8.91015 20 12.5C20 16.0899 17.0899 19 13.5 19H6"
						stroke="white"
						stroke-width="2"
						stroke-linecap="square"
					></path></svg
				>
			</span>
		{:else}
			<a style="color: {color}" href="./{previous}" aria-label="Previous">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					viewBox="0 0 24 24"
					fill="none"
					class="x47corl x1b1b1ih x98svd5 xnnoyiz"
					><path
						d="M7 3L4 6L7 9M5 6H13.5C17.0899 6 20 8.91015 20 12.5C20 16.0899 17.0899 19 13.5 19H6"
						stroke="white"
						stroke-width="2"
						stroke-linecap="square"
					></path></svg
				>
			</a>
		{/if}
		{#if nextDisabled}
			<span class="disabled">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					width="24"
					height="24"
					viewBox="0 0 24 24"
					fill="none"
					class="x47corl x1b1b1ih x98svd5 xnnoyiz"
					><path
						d="M10 20L14 4M18 8.00004L22 12L18 16M6 16L2 12L6 8.00004"
						stroke="white"
						stroke-width="2"
						stroke-linecap="square"
					></path></svg
				>
			</span>
		{:else}
			<a style="color: {color}" href="./{next}" aria-label="Next">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					viewBox="0 0 24 24"
					fill="none"
					class="x47corl x1b1b1ih x98svd5 xnnoyiz"
					><path
						d="M17 3L20 6L17 9M19 6H10.5C6.91015 6 4 8.91015 4 12.5C4 16.0899 6.91015 19 10.5 19H18"
						stroke="white"
						stroke-width="2"
						stroke-linecap="square"
					></path></svg
				>
			</a>
		{/if}
		<a style="color: {color}" href="https://github.com/m2rtv/advent-of-js-2024" aria-label="Next">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				width="24"
				height="24"
				viewBox="0 0 24 24"
				fill="none"
				class="x47corl x1b1b1ih x98svd5 xnnoyiz"
				><path
					d="M10 20L14 4M18 8.00004L22 12L18 16M6 16L2 12L6 8.00004"
					stroke="white"
					stroke-width="2"
					stroke-linecap="square"
				></path></svg
			>
		</a>
	</div>
</footer>

<style>
	footer {
		width: 100%;
		position: fixed;
		bottom: 40px;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.footer-menu {
		height: 3.5rem;
		display: flex;
		align-items: center;
		gap: 1rem;
		padding-left: 1.25rem;
		padding-right: 1.25rem;
		background-color: var(--brand-color);
		border-radius: var(--radius-lg);
	}
	a svg,
	.disabled svg {
		width: 24px;
		height: 24px;
	}
	.disabled {
		opacity: 0.5;
	}
</style>
