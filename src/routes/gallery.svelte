<script lang="ts">
	import axios from 'axios';
	import { onMount } from 'svelte';

	let term = '';
	let photos: {
		id: string;
		alt_description: string;
		urls: { regular: string };
		likes: string;
		user: {
			name: string;
			profile_image: { medium: string };
		};
	}[] = [];
	let searchWord = 'Benoit';

	const fetchdata = async () => {
		const response = await axios.get(
			`https://api.unsplash.com/search/photos?page=1&query=${
				term || 'mac'
			}&client_id=EhbSp_LlOS5xru-qq1DbGfh01odyN7IsQtHFv7BW0o8`
		);
		photos = response.data.results;
	};

	onMount(() => {
		fetchdata();
	});

	const validate = (event : string) => {
		console.log(event);
	};
</script>

<div class="container">
	<div class="header">
		<h1>Image Gallery</h1>
		<form class="input-container" on:submit|preventDefault={validate}>
			<input type="text" class="input" name="searchWord" id="searchWord" bind:value={searchWord} />
			<button type="submit" class="button">Search</button>
		</form>
	</div>
	<div class="photos">
		{#each photos as photo, i (photo.id)}
			<div class="img-container">
				<img src={photo.urls.regular} alt={photo.alt_description} class="image" />
				<p>{photo.likes}</p>
				<div class="info">
					<img src={photo.user.profile_image.medium} alt={photo.user.name} />
					<p>{photo.user.name}</p>
				</div>
			</div>
		{/each}
	</div>
</div>

<style>
	.image {
		height: 25vh;
		margin: 2px;
	}
	.photos {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-evenly;
	}
	.container {
		width: 1230px;
		margin: 0 auto;
	}
	.header {
		text-align: center;
		font-size: 20px;
	}
	.input {
		padding: 10px;
		width: 400px;
		border-radius: 10px;
		outline: none;
		border: 1px solid gray;
		font-size: 20px;
	}
	.button {
		padding: 10px;
		font-size: 20px;
		background-color: aqua;
		border-radius: 10px;
		border: none;
		color: white;
	}
	.input-container {
		margin-bottom: 40px;
	}

	.info {
		display: flex;
	}
</style>
