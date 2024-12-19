<script lang="ts">
	import Icon from '$lib/components/ui/Icon/Icon.svelte';
	import Footer from '$lib/components/Footer.svelte';

	interface Tag {
		id: number;
		text: string;
	}

	let text = $state('');
	let tags = $state<Tag[]>([]);

	function removeTag(id: number) {
		tags = tags.filter((tag) => tag.id !== id);
	}

	function handleInput(event: KeyboardEvent) {
		const value = (event.target as HTMLInputElement).value;

		if (event.key === 'Backspace' && !value) {
			if (tags.length > 0) {
				tags = tags.slice(0, -1);
			}
			return;
		}

		if (value.endsWith(',') || event.key === 'Enter') {
			const tagTexts = value
				.split(',')
				.map((t) => t.trim())
				.filter(Boolean);

			if (tagTexts.length) {
				const lastId = tags.length ? Math.max(...tags.map((t) => t.id)) : 0;
				const newTags = tagTexts.map((text, index) => ({
					id: lastId + index + 1,
					text
				}));

				tags = [...tags, ...newTags];
				text = '';
			}
		}
	}
</script>

<div class="wrapper">
	<div class="tags-input-wrapper">
		{#each tags as tag (tag.id)}
			<div class="tag-badge">
				<span>{tag.text}</span>
				<button class="tags-badge-icon" onclick={() => removeTag(tag.id)}>
					<Icon iconName="cross-large" size="12" />
				</button>
			</div>
		{/each}
		<input
			bind:value={text}
			type="text"
			placeholder="Add tags, separated by a comma"
			autocomplete="off"
			oninput={handleInput}
			onkeydown={handleInput}
		/>
	</div>
</div>

<Footer previous="07" next="09" />

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
		/* font-size: 0.75rem; */
	}

	.tags-input-wrapper {
		width: min(100%, var(--width-md));
		display: flex;
		align-items: center;
		flex-wrap: wrap;
		gap: 0.5rem 0.5rem;
		padding: 0.5rem 0.5rem 0.5rem 0.5rem;
		min-height: 2.5rem;
		border-radius: var(--radius);
		border: 1px solid var(--brand-color-200);
		background-color: #fff;
	}
	.tags-input-wrapper:focus-within {
		border: 1px solid var(--brand-color-600);
	}

	.tag-badge {
		height: 1.35rem;
		padding: 0 0 0 0.5rem;
		display: flex;
		align-items: center;
		background-color: var(--brand-color-100);
		border-radius: var(--radius);
		cursor: pointer;
	}

	.tag-badge span {
		white-space: nowrap;
	}

	.tags-badge-icon {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 1.5rem;
		height: 1.5rem;
	}

	.wrapper input {
		min-width: 214px;
		flex: 1;
		border-width: 0px;
		height: auto;
		padding-right: 0px;
		padding-left: 0rem;
	}
</style>
