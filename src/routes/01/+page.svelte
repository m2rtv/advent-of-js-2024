<script lang="ts">
	import Icon from '$lib/components/ui/Icon/Icon.svelte';
	import Footer from '$lib/components/Footer.svelte';
	let showPassword = false;

	const symbolsArray = ['•', '*', ' ', ' '];
	const lettersArray = [
		'%',
		'#',
		'*',
		'=',
		':',
		'.',
		'a',
		'b',
		'c',
		'd',
		'e',
		'f',
		'g',
		'h',
		'i',
		'j',
		'k',
		'l',
		'm',
		'n',
		'o',
		'p',
		'q',
		'r',
		's',
		't',
		'u',
		'v',
		'w',
		'x',
		'y',
		'z',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' ',
		' '
	];

	function getRandomSymbol(): string {
		const array = showPassword ? lettersArray : symbolsArray;
		return array[Math.floor(Math.random() * array.length)];
	}

	let gridItems: string[] = [];
	let gridContainer: HTMLDivElement;

	function updateGrid() {
		if (!gridContainer) return;

		const padding = window.innerWidth < 720 ? 64 : 160; // 32px * 2 or 80px * 2
		const containerWidth = gridContainer.clientWidth - padding;
		const containerHeight = gridContainer.clientHeight - padding;
		const columns = Math.floor(containerWidth / 16);
		const rows = Math.floor(containerHeight / 16);

		gridItems = Array(columns * rows)
			.fill('')
			.map(() => getRandomSymbol());
	}

	$: showPassword, updateGrid();
</script>

<div class="grid-bg" bind:this={gridContainer} use:updateGrid>
	{#each gridItems as symbol}
		<div class="grid-item">{symbol}</div>
	{/each}
</div>
<div class="wrapper">
	<div class="form">
		<div>
			<input placeholder="Email" type="email" id="email" name="email" autocomplete="off" required />
		</div>
		<div>
			<div class="password-input-wrapper">
				<input
					placeholder="Password"
					type={showPassword ? 'text' : 'password'}
					id="pass"
					name="password"
					minlength="8"
					autocomplete="off"
					required
				/>
				<button type="button" on:click={() => (showPassword = !showPassword)}>
					<Icon iconName={showPassword ? 'eye-open' : 'eye-slash'} />
				</button>
			</div>
		</div>
	</div>
</div>

<Footer previous="" next="02" />

<style>
	.wrapper {
		min-height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 0 1rem;
		background-color: var(--brand-color-bg);
		z-index: -1;
	}
	.form {
		display: grid;
		gap: 1rem;
		width: min(100%, var(--width-sm));
		z-index: 1;
	}

	.password-input-wrapper input {
		width: 100%;
		border-width: 0px;
		padding-right: 0px;
	}
	.password-input-wrapper {
		display: flex;
		align-items: center;
		background-color: #fff;
		padding-right: 1rem;
		border-radius: var(--radius);
		border: 1px solid var(--brand-color-200);
	}
	.password-input-wrapper:focus-within {
		border: 1px solid var(--brand-color-600);
	}
	.grid-bg {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100vh;
		padding: 80px;
		display: grid;
		grid-template-columns: repeat(auto-fill, 16px);
		grid-template-rows: repeat(auto-fill, 16px);
		gap: 0;
		color: #ffffff;
		pointer-events: none;
	}

	@media (max-width: 720px) {
		.grid-bg {
			padding: 32px;
		}
	}

	.grid-item {
		width: 16px;
		height: 16px;
		display: flex;
		align-items: center;
		justify-content: center;
		font-family: monospace;
		font-size: 12px;
	}
</style>
