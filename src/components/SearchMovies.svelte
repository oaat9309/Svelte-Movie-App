<script>
	import { goto } from '$app/navigation';
	import { fly } from 'svelte/transition';
	let inputValue = '';
	let active = false;
	function cancelInactive() {
		if (inputValue) {
			active = true;
		} else {
			active = false;
		}
	}

	function submitSearch(e) {
		e.preventDefault();
		goto('/search/' + inputValue);
	}
</script>

<form on:submit={submitSearch} class="search">
	{#if !active}
		<label in:fly={{ y: -10, duration: 500 }} out:fly={{ y: 10, duration: 500 }} for="search-movie"
			>Search Movie</label
		>
	{/if}
	<input
		on:focus={() => (active = true)}
		on:blur={cancelInactive}
		bind:value={inputValue}
		class={active ? 'selected' : ''}
		type="text"
		name="search-movie"
	/>
	{#if inputValue}
		<button in:fly={{ x: 0, duration: 500 }} out:fly={{ x: 20, duration: 500 }}>Search</button>
	{/if}
</form>

<style>
	.search {
		position: relative;
		width: 100%;
		margin: 1rem;
	}
	button {
		font-size: 0.7rem;
		padding: 0rem 1rem;
		background: rgb(96, 110, 201);
		color: white;
		font-weight: bold;
		border: none;
		position: absolute;
		bottom: 50%;
		right: 0;
		transform: translate(0, 50%);
		height: 100%;
		border-top-right-radius: 10px;
		border-bottom-right-radius: 10px;
		cursor: pointer;
	}
	button:hover {
		background: rgb(33, 56, 207);
	}
	input {
		width: 100%;
		border: none;
		font-size: 1rem;
		outline: none;
		color: rgb(255, 255, 255);
		padding: 0.5rem 0.1rem;
		transition: background 0.75s ease-out;
		font-weight: bold;
		background: rgb(63, 63, 63);
		border-radius: 10px;
		padding: 1rem;
		align-self: center;
	}
	input.selected {
		background: rgb(50, 50, 50);
	}
	label {
		position: absolute;
		font-size: 0.8rem;
		top: 50%;
		left: 0;
		transform: translate(0, -50%);
		pointer-events: none;
		color: #fff;
		padding: 0rem 1rem;
	}
</style>
