<script lang="ts">
	import movieData from './top-100-christmas-movies.json';

	interface Movie {
		Title: string;
		Year: string;
		Image: string;
		'Image Alt': string;
	}

	interface Props {
		movies: Movie[];
		onkeydown?: (event: KeyboardEvent) => void;
	}

	let props = $props<Props>();
	let selected: Movie | null = $state(null);
	// Randomly selected background when no movie is selected
	let backgroundMovie = movieData[Math.floor(Math.random() * movieData.length)];
	let selectExpanded = $state(false);
	let searchQuery = $state('');
	let focusedIndex = $state(-1);

	let filteredMovies = $derived.by(() => {
		return movieData.filter((movie) =>
			movie.Title.toLowerCase().includes(searchQuery.toLowerCase())
		);
	});

	function handleMovieSelect(movie: Movie) {
		selected = movie;
		selectExpanded = false;
		focusedIndex = -1;
	}

	function handleKeydown(event: KeyboardEvent) {
		if (!selectExpanded) return;

		switch (event.key) {
			case 'ArrowDown':
				event.preventDefault();
				focusedIndex = Math.min(focusedIndex + 1, filteredMovies.length - 1);
				scrollIntoView(focusedIndex);
				break;
			case 'ArrowUp':
				event.preventDefault();
				focusedIndex = Math.max(focusedIndex - 1, 0);
				scrollIntoView(focusedIndex);
				break;
			case 'Enter':
				event.preventDefault();
				if (focusedIndex >= 0 && focusedIndex < filteredMovies.length) {
					handleMovieSelect(filteredMovies[focusedIndex]);
				}
				break;
			case 'Escape':
				selectExpanded = false;
				focusedIndex = -1;
				break;
		}
	}

	function scrollIntoView(index: number) {
		const movieList = document.querySelector('.movie-list');
		const movieItem = movieList?.children[index] as HTMLElement;
		if (movieItem) {
			movieItem.scrollIntoView({ block: 'nearest' });
		}
	}

	function clickOutside(node: HTMLElement) {
		$effect(() => {
			const handleClick = (event: MouseEvent) => {
				if (!node.contains(event.target as Node)) {
					selectExpanded = false;
					focusedIndex = -1;
				}
			};

			document.addEventListener('click', handleClick);

			return () => {
				document.removeEventListener('click', handleClick);
			};
		});
	}

	// Reset focused index when expanding/collapsing or searching
	$effect(() => {
		if (!selectExpanded) focusedIndex = -1;
	});

	$effect(() => {
		if (searchQuery !== '') focusedIndex = -1;
	});
</script>

<div
	class="background"
	style:background-image={`url(${selected?.Image || backgroundMovie.Image})`}
></div>

<div class="wrapper">
	<div class="select-wrapper" use:clickOutside>
		<button
			class="custom-select"
			onclick={() => (selectExpanded = !selectExpanded)}
			onkeydown={handleKeydown}
			{...props}
		>
			{#if selected}
				<div>
					<span class="title">{selected.Title}</span>
					<span class="year">({selected.Year})</span>
				</div>
			{:else}
				<span>Select your favourite holiday movie</span>
			{/if}
			<span class={selectExpanded ? 'menu-open' : 'menu-closed'}>🎄</span>
		</button>

		{#if selectExpanded}
			<div class="movie-list-wrapper">
				<input
					type="text"
					placeholder="Search movies..."
					bind:value={searchQuery}
					onkeydown={handleKeydown}
				/>
				<div class="movie-list">
					{#each filteredMovies as movie, index}
						<button
							class="movie-item"
							onclick={() => handleMovieSelect(movie)}
							class:selected={selected?.Title === movie.Title}
							class:focused={index === focusedIndex}
							onkeydown={handleKeydown}
						>
							<img src={movie.Image} alt={movie['Image Alt']} />
							<div class="movie-info">
								<div class="title">{movie.Title}</div>
								<div class="year">{movie.Year}</div>
							</div>
						</button>
					{/each}
				</div>
			</div>
		{/if}
	</div>
</div>

<style>
	.background {
		position: fixed;
		top: -64px;
		left: -64px;
		width: calc(100% + 128px);
		height: calc(100% + 128px);
		background-size: cover;
		background-position: center;
		filter: blur(128px);
		z-index: -1;
		transition: background-image 0.4s ease;
		transform: scale(1.1);
	}

	.wrapper {
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-top: 1rem;
		position: relative;
		font-size: 0.875rem;
		padding-left: 1rem;
		padding-right: 1rem;
	}

	.select-wrapper {
		width: min(100%, 420px);
		position: relative;
	}

	input {
		all: unset;
		width: 100%;
		height: 2rem;
		display: flex;
		align-items: center;
		background-color: #ffffff;
		border-radius: 6px;
		box-sizing: border-box;
		padding-left: 1rem;
		padding-right: 0.5rem;
		margin-bottom: 1rem;
	}

	button {
		all: unset;
		width: 100%;
		box-sizing: border-box;
	}

	.custom-select {
		display: flex;
		align-items: center;
		justify-content: space-between;
		height: 3rem;
		width: 100%;
		padding-right: 0.5rem;
		background: rgba(255, 255, 255, 0.75);
		padding: 1.5rem;
		border-radius: 999px;
		backdrop-filter: blur(8px);
		cursor: pointer;
	}

	.menu-open {
		font-size: 1rem;
		transform: rotate(0);
		transition: transform 0.3s ease;
	}

	.menu-closed {
		font-size: 1rem;
		transform: rotate(180deg);
		transition: transform 0.3s ease;
	}

	.movie-list-wrapper {
		position: relative;
		top: 0.5rem;
		background: rgba(255, 255, 255, 0.75);
		border-radius: 1.5rem;
		overflow: hidden;
		padding: 1.5rem;
	}

	.movie-list {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
		max-height: 420px;
		overflow-y: scroll;
		scrollbar-width: none; /* Firefox */
		-ms-overflow-style: none; /* IE and Edge */
	}

	/* Chrome, Safari and Opera */
	.movie-list::-webkit-scrollbar {
		display: none;
	}

	.movie-item {
		display: flex;
		align-items: center;
		gap: 1rem;
		cursor: pointer;
		padding: 0.5rem;
		border-radius: 6px;
		transition: background-color 0.2s ease;
	}

	.movie-item:hover {
		background-color: rgba(255, 255, 255, 0.5);
	}

	.movie-item img {
		width: 48px;
		border-radius: 4px;
	}

	.title {
		font-weight: 600;
	}
	.year {
		font-weight: 400;
		color: rgba(0, 0, 0, 0.5);
	}

	.selected {
		background-color: rgba(255, 255, 255, 1);
	}

	.focused {
		background-color: rgba(255, 255, 255, 0.5);
	}
</style>
