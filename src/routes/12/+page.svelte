<script lang="ts">
	import { onMount } from 'svelte';
	import Footer from '$lib/components/Footer.svelte';

	let randomQuote = $state<string>('');
	let randomXPos = $state<number>(0);
	let randomYPos = $state<number>(0);
	let randomRotation = $state<number>(0);

	const fetchUrl = async (url: string) => {
		const response = await fetch(url);
		const data = await response.json();
		return data;
	};

	onMount(async () => {
		randomXPos = Math.floor(Math.random() * (document.body.clientWidth - 200)) + 100;
		randomYPos = Math.floor(Math.random() * (document.body.clientHeight / 4));
		randomRotation = Math.floor(Math.random() * 180) - 90;
		console.log(randomXPos, randomYPos);
		fetchUrl('https://api.kanye.rest/quotes').then((data) => {
			const randomIndex = Math.floor(Math.random() * data.length);
			randomQuote = data[randomIndex === 0 ? 1 : randomIndex];
		});
	});
</script>

<div class="wrapper">
	<div class="text-area-container">
		<h1>
			{randomQuote}
		</h1>
		<p>— Kanye West</p>
	</div>
</div>
<img
	src="/yeezy.png"
	alt="Yeezy"
	style="left: {randomXPos}px; top: {randomYPos}px; transform: rotate({randomRotation}deg);"
/>

<Footer previous="09" next="15" />

<style>
	.wrapper {
		width: 100%;
		height: 100vh;
		padding: 0 1rem;
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: var(--brand-color-100);
		overflow: hidden;
		position: relative;
	}

	.text-area-container {
		width: min(100%, 640px);
		text-align: center;
		position: relative;
		z-index: 2;
	}
	.text-area-container h1 {
		font-size: 2rem;
		z-index: 1;
	}
	.text-area-container p {
		margin-top: 2rem;
		font-size: 1rem;
		z-index: 1;
	}

	img {
		width: 80px;
		position: absolute;
		z-index: 1;
	}
	@media (min-width: 640px) {
		.text-area-container h1 {
			font-size: 3rem;
		}
		img {
			width: 120px;
		}
	}
</style>
