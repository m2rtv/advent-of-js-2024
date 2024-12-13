<script lang="ts">
	import Icon from '$lib/components/ui/Icon/Icon.svelte';
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
			<label for="email">Email:</label>
			<input type="email" id="email" name="email" autocomplete="off" required />
		</div>
		<div>
			<label for="pass">Password:</label>
			<div class="password-input-wrapper">
				<input
					type={showPassword ? 'text' : 'password'}
					id="pass"
					name="password"
					minlength="8"
					autocomplete="off"
					required
				/>
				<button type="button" on:click={() => (showPassword = !showPassword)}>
					<Icon iconName={showPassword ? 'eye-open' : 'eye-closed'} />
				</button>
			</div>
		</div>
		<!-- <div class="login-button">
			<button type="submit">Login</button>
		</div> -->
	</div>
</div>

<style>
	label {
		color: #ffffff;
	}
	.wrapper {
		min-height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: #151515;
		z-index: -1;
		font-family: monospace;
		font-size: 0.75rem;
	}
	.form {
		display: grid;
		gap: 1rem;
		width: min(100%, 320px);
		z-index: 1;
	}
	input {
		all: unset;
		box-sizing: border-box;
		width: 100%;
		height: 2rem;
		display: flex;
		align-items: center;
		background-color: #fff;
		padding-left: 0.5rem;
		padding-right: 0.5rem;
	}
	.password-input-wrapper input {
		all: unset;
		width: 100%;
		height: 100%;
	}
	.password-input-wrapper {
		height: 2rem;
		display: flex;
		align-items: center;
		background-color: #fff;
		padding-left: 0.5rem;
		padding-right: 0.25rem;
	}
	.login-button button {
		all: unset;
		box-sizing: border-box;
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 2rem;
		background-color: var(--brand-color);
		color: #ffffff;
		border-radius: var(--radius);
		padding-left: 0.5rem;
		padding-right: 0.5rem;
		font-weight: 500;
		margin-top: 1.25rem;
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
		color: #ffffff20;
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
