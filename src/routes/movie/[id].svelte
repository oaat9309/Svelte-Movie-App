<script context="module">
	import { fly } from 'svelte/transition';
	// params가 url의 id값을 불러와줍니다
	export async function load({ fetch, params }) {
		const res = await fetch(
			`https://api.themoviedb.org/3/movie/${params.id}?api_key=${
				import.meta.env.VITE_API
			}&language=en-US`
		);
		const movieDetail = await res.json();
		console.log(movieDetail);

		if (res.ok) {
			return {
				props: { movieDetail }
			};
		}
	}
</script>

<script>
	export let movieDetail;
	import.meta.env.VITE_API;
</script>

<div
	class="movie-details"
	in:fly={{ y: 50, duration: 500, delay: 500 }}
	out:fly={{ duration: 500 }}
>
	<div class="img-container">
		<img
			src={'https://image.tmdb.org/t/p/original' + movieDetail.backdrop_path}
			alt={movieDetail.title}
		/>
	</div>
	<div class="txt-container">
		<h1>{movieDetail.title}</h1>
		<p class="overview">{movieDetail.overview}</p>
		<p>
			<span>Release Date: </span>
			{movieDetail.release_date || '--'} <br />
			<span>Runtime: </span>
			{movieDetail.runtime + 'm' || '--'} <br />
			<span>Rating: </span>
			{movieDetail.vote_count
				? movieDetail.vote_average + ' (' + movieDetail.vote_count + ')'
				: 'Not Available'}
		</p>
	</div>
</div>

<style>
	h1 {
		padding: 1rem 0rem 1rem;
	}
	p {
		padding: 1rem 0rem;
	}
	.img-container {
		width: 100%;
	}
	img {
		width: 100%;
		height: auto;
		border-radius: 1rem;
	}
	.movie-details {
		margin: 2rem 20%;
	}
	span {
		font-weight: bold;
	}
</style>
