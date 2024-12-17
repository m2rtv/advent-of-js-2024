<script lang="ts">
	interface IconProps {
		iconName?: string;
		size?: string;
		color?: string;
		textIconName?: string;
		class?: string;
	}

	let {
		size = '20',
		color = '#000000',
		iconName,
		textIconName = 'W',
		class: className = ''
	}: IconProps = $props();

	// Using dynamic imports for SVG files
	const loadIcon = async (iconName: string) => {
		try {
			const icon = await import(`$lib/icons/${iconName}.svg?raw`);
			return icon.default;
		} catch (error) {
			console.error(
				`Failed to load icon: "${iconName}". Please verify that the icon name matches one of the icons in our library.`
			);
			return null;
		}
	};
</script>

<!--
@component
An icon component, where **iconName="add"** prop uses **add.svg** icon from the assets directory.  
Size and color use the default css variable values from Figma (with dynamic theme colors).

Usage:
  ```tsx
  <Icon iconName="add" />
  <Icon textIconName="H" size="32" color="red" />
  ```
-->

<div
	class="icon-wrapper {className}"
	style:width={size + 'px'}
	style:height={size + 'px'}
	style:--icon-color={color}
>
	{#if iconName}
		{#await loadIcon(iconName) then svg}
			{#if svg}
				{@html svg}
			{:else}
				<span>🙈</span>
			{/if}
		{:catch error}
			<span>Failed to load icon</span>
		{/await}
	{:else}
		<span class="text-icon">{textIconName}</span>
	{/if}
</div>

<style>
	.icon-wrapper {
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.icon-wrapper :global(svg) {
		width: 100%;
		height: 100%;
	}
	.icon-wrapper :global(svg path) {
		fill: var(--icon-color);
	}
	.text-icon {
		line-height: 1;
		color: #000000;
		user-select: none;
	}
</style>
