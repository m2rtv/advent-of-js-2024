<script lang="ts">
	import YoutubeEmbed from './YoutubeEmbed.svelte';
	import Footer from '$lib/components/Footer.svelte';
</script>

<div class="wrapper">
	<YoutubeEmbed url="https://www.youtube.com/embed/L_LUpnjgPso?si=F07Dj06o3mXgcCPD" />
</div>

<Footer previous="14" next="16" />

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
</style>
