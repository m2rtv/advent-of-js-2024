<script lang="ts">
	interface Props {
		url: string;
		title?: string;
		width?: number;
		height?: number;
		aspectRatio?: string;
	}

	let { url, title = 'Video player', width = 560, height = 315, aspectRatio }: Props = $props();

	const calculatedAspectRatio = aspectRatio || `${width} / ${height}`;
</script>

<div class="video-container" style:aspect-ratio={calculatedAspectRatio}>
	<iframe
		{title}
		src={url}
		allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
		referrerpolicy="strict-origin-when-cross-origin"
		allowfullscreen
	></iframe>
</div>

<style>
	.video-container {
		width: 100%;
		position: relative;
	}

	iframe {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		border: 0;
	}
</style>
