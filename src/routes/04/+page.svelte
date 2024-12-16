<script lang="ts">
	import Footer from '$lib/components/Footer.svelte';

	let isResizing = $state(false);
	let startY = $state(0);
	let startX = $state(0);
	let topHeight = $state(200);
	let leftWidth = $state(300);
	let resizeType = $state<'vertical' | 'horizontal' | null>(null);

	$effect(() => {
		if (!isResizing) return;

		const handleVerticalResize = (e: MouseEvent) => {
			if (resizeType !== 'vertical') return;
			const dy = e.clientY - startY;
			topHeight = Math.max(100, topHeight + dy);
			startY = e.clientY;
		};

		const handleHorizontalResize = (e: MouseEvent) => {
			if (resizeType !== 'horizontal') return;
			const dx = e.clientX - startX;
			leftWidth = Math.max(100, leftWidth + dx);
			startX = e.clientX;
		};

		const stopResize = () => {
			isResizing = false;
			resizeType = null;
		};

		document.addEventListener('mousemove', handleVerticalResize);
		document.addEventListener('mousemove', handleHorizontalResize);
		document.addEventListener('mouseup', stopResize);

		return () => {
			document.removeEventListener('mousemove', handleVerticalResize);
			document.removeEventListener('mousemove', handleHorizontalResize);
			document.removeEventListener('mouseup', stopResize);
		};
	});
</script>

<div class="wrapper">
	<div class="panel-container">
		<div class="top-panel" style="height: {topHeight}px">
			<div class="content">Top Panel</div>
			<button
				class="resize-handle-horizontal"
				aria-label="Resize top panel"
				onmousedown={(e) => {
					isResizing = true;
					resizeType = 'vertical';
					startY = e.clientY;
				}}
			></button>
		</div>
		<div class="bottom-container">
			<div class="left-panel" style="width: {leftWidth}px">
				<div class="content">Left Panel</div>
				<button
					class="resize-handle-vertical"
					aria-label="Resize left panel"
					onmousedown={(e) => {
						isResizing = true;
						resizeType = 'horizontal';
						startX = e.clientX;
					}}
				></button>
			</div>
			<div class="right-panel">
				<div class="content">Right Panel</div>
			</div>
		</div>
	</div>
</div>

<Footer color="#fff" previous="03" next="05" />

<style>
	.wrapper {
		width: 100%;
		height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.panel-container {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		position: relative;
	}

	.top-panel {
		background-color: #f0f0f0;
		position: relative;
		width: 100%;
		min-height: 100px;
	}

	.bottom-container {
		flex: 1;
		display: flex;
		position: relative;
	}

	.left-panel {
		background-color: #e0e0e0;
		position: relative;
		min-width: 100px;
	}

	.right-panel {
		background-color: #d0d0d0;
		flex: 1;
	}

	.resize-handle-horizontal {
		position: absolute;
		bottom: 0;
		width: 100%;
		height: 6px;
		background: transparent;
		cursor: ns-resize;
		border: none;
		padding: 0;
	}

	.resize-handle-vertical {
		position: absolute;
		right: 0;
		top: 0;
		width: 6px;
		height: 100%;
		background: transparent;
		cursor: ew-resize;
	}

	.content {
		padding: 1rem;
	}

	.resize-handle-horizontal:hover {
		background: rgba(255, 255, 255, 0.5);
	}

	.resize-handle-vertical:hover {
		background: rgba(255, 255, 255, 0.5);
	}
</style>
