<script lang="ts">
	import Footer from '$lib/components/Footer.svelte';

	const challenges = [
		{
			day: '01',
			name: 'Show / hide password',
			completed: true,
			description:
				'Create a password input field. Clicking on the eyeball will toggle the password visibility on and off. I did not bother with the svg animation and just played with the background elements.'
		},
		{
			day: '02',
			name: 'Custom combobox / dropdown',
			completed: true,
			description:
				'Custom combobox / dropdown 🎄🎄🎄 with a search field. Got a bit messy and used AI for some things, but it works.'
		},
		{ day: '03', name: 'Resizable text area', completed: true, description: 'Resizable text area' },
		{
			day: '04',
			name: 'Resizable splitpanels',
			completed: true,
			description: 'Resizable splitpanels'
		},
		{
			day: '05',
			name: 'Character counter',
			completed: true,
			description:
				'Character counter. By far the easiest so far — just binding text with the input and using derived to show text.length.'
		},
		{
			day: '06',
			name: 'Copy to clipboard',
			completed: true,
			description: 'An input with a copy to clipboard button and a tooltip.'
		},
		{
			day: '07',
			name: 'Slugify input',
			completed: true,
			description: 'Slugify text from the input to a url slug.'
		},
		{
			day: '08',
			name: 'Tag input editor',
			completed: true,
			description:
				'By default, the input looks like a normal text input. When the user enters some text and then types a comma, the text turns into a tag. The user can enter as many tags as they’d like. If there isn’t any text in the input and the user hits the delete key, then it will delete the last tag in the list.'
		},
		{
			day: '09',
			name: 'Persistent form data',
			completed: true,
			description:
				'Before the user submits the form, they should be able to refresh the page or visit another page, come back, and see all their form data. <br/>Once the user submits the form, the form data should clear'
		},
		{
			day: '12',
			name: 'Quote generator',
			completed: true,
			description: 'Used the Kanye Rest API here instead.'
		},
		{
			day: '15',
			name: 'Responsive Youtube embed',
			completed: true,
			description:
				'A simple responsive full-width Youtube embed component that respects the aspect ratio.'
		}
	];
</script>

<div class="wrapper">
	<div class="content">
		<h1>Advent of Javascript 2024</h1>
		<p>
			A 24-day JS challenge created by Amy Dutton. I'm not taking the challenge, or the prompts too
			seriously — just having some fun, doing select few, and modifying the prompts how I see fit ✌️ <br
			/>
			Using Svelte 5.
		</p>
		<div class="challenges-list">
			{#each challenges as { day, name, completed, description }}
				<div class="challenge-item">
					<span class="challenge-day">{day}.12.2024</span>
					<div>
						<a href="/{day}" class="challenge-link" class:completed class:pending={!completed}>
							{name}
						</a>
						<p class="challenge-description">{@html description}</p>
					</div>
				</div>
			{/each}
		</div>
	</div>
</div>

<Footer previousDisabled next="01" />

<style>
	:global(body) {
		background-color: white;
	}

	.wrapper {
		width: 100%;
		display: flex;
		justify-content: center;
		margin-bottom: 100px;
	}
	.content {
		display: grid;
		align-items: center;
		justify-content: center;
		max-width: 600px;
		gap: 2rem;
		padding: 2rem;
		font-size: 0.875rem;
	}

	.challenges-list {
		display: grid;
		gap: 2rem;
	}

	.challenge-item {
		display: grid;
		grid-template-columns: 1fr;
	}

	@media (min-width: 600px) {
		.challenge-item {
			grid-template-columns: 1fr 3fr;
			gap: 1rem;
		}
	}

	.challenge-day {
		color: var(--color-text-muted-light);
	}

	.challenge-link {
		text-decoration: none;
		transition: all 0.2s ease;
		font-weight: 500;
	}

	.challenge-description {
		color: var(--color-text-muted);
	}

	.completed {
		color: var(--color-text-black);
	}

	.pending {
		color: #666;
		pointer-events: none;
	}

	.completed:hover {
		opacity: 0.7;
	}
</style>
