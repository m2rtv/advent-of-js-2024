<script lang="ts">
	import Footer from '$lib/components/Footer.svelte';
	import { browser } from '$app/environment';

	interface StoredUser {
		name: string;
		email: string;
		movies: Record<string, boolean>;
	}

	interface Movie {
		id: string;
		title: string;
		year: number;
	}

	const moviesList: Movie[] = [
		{ id: 'elf', title: 'Elf', year: 2003 },
		{ id: 'home-alone', title: 'Home Alone', year: 1990 },
		{ id: 'the-grinch', title: 'The Grinch', year: 1966 },
		{ id: 'its-a-wonderful-life', title: 'Its a Wonderful Life', year: 1946 },
		{ id: 'die-hard', title: 'Die Hard', year: 1988 }
	];

	// Initialize with empty values
	let name = $state('');
	let email = $state('');
	let movies = $state<Record<string, boolean>>({});

	// Load data from sessionStorage
	if (browser) {
		const stored = sessionStorage.getItem('user');
		if (stored) {
			const data: StoredUser = JSON.parse(stored);
			name = data.name;
			email = data.email;
			movies = data.movies || {};
		} else {
			const initialData = () => ({ name: '', email: '', movies: {} });
			sessionStorage.setItem('user', JSON.stringify(initialData()));
		}
	}

	function toggleMovie(title: string) {
		movies = {
			...movies,
			[title]: !movies[title]
		};
	}

	// Save to sessionStorage whenever any value changes
	$effect(() => {
		if (browser) {
			sessionStorage.setItem('user', JSON.stringify({ name, email, movies }));
		}
	});

	function handleSubmit(e: SubmitEvent) {
		e.preventDefault();
		if (browser) {
			sessionStorage.removeItem('user');
			// Reset state
			name = '';
			email = '';
			movies = {};
		}
	}
</script>

<div class="wrapper">
	<form onsubmit={handleSubmit}>
		<div>
			<input
				placeholder="Name"
				type="text"
				id="name"
				name="name"
				bind:value={name}
				autocomplete="off"
				required
			/>
		</div>
		<div>
			<input
				placeholder="Email"
				type="email"
				id="email"
				name="email"
				bind:value={email}
				autocomplete="off"
				required
			/>
		</div>
		<fieldset>
			<div>
				<legend>What holiday movies have you watched this year?</legend>
			</div>

			{#each moviesList as movie}
				<div class="checkbox-item">
					<input
						type="checkbox"
						id={movie.id}
						name={movie.id}
						checked={movies[movie.title] || false}
						onchange={() => toggleMovie(movie.title)}
					/>
					<label for={movie.id}>{movie.title} ({movie.year})</label>
				</div>
			{/each}
		</fieldset>
		<button type="submit">Submit</button>
	</form>
</div>

<Footer previous="08" next="12" />

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
	form {
		display: grid;
		gap: 1rem;
		width: min(100%, var(--width-sm));
		z-index: 1;
	}
	.checkbox-item {
		display: flex;
		align-items: center;
		gap: 0.5rem;
	}

	button {
		all: unset;
		cursor: pointer;
		font-weight: 500;
		width: 100%;
		height: 2.5rem;
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: var(--brand-color);
		color: #fff;
		border-radius: var(--radius);
		margin-top: 1rem;
	}
</style>
