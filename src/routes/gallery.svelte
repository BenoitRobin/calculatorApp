<script lang="ts">
	import axios from 'axios';
	import { onMount } from 'svelte';
	import { fly, fade } from 'svelte/transition';

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

	const fetchdata = async () => {
		const response = await axios.get(
			`https://api.unsplash.com/search/photos?page=1&query=${
				term || 'bike'
			}&client_id=EhbSp_LlOS5xru-qq1DbGfh01odyN7IsQtHFv7BW0o8`
		);
		photos = response.data.results;
	};

	onMount(() => {
		fetchdata();
	});

	const handleSearch = async () => {
		if (!term) return;
		await fetchdata();
		term = '';
	};
</script>

<div class="container">
	<div class="header">
		<h1>Image Gallery</h1>
		<form class="input-container">
			<input type="text" class="input" name="searchWord" id="searchWord" bind:value={term} />
			<button on:click={handleSearch} class="button">Search</button>
		</form>
	</div>
	<div class="photos">
		{#each photos as photo, i (photo.id)}
			<div class="img-container" in:fly={{ y: 500, duration: 3000, delay: i * 200 }} out:fade>
				<img src={photo.urls.regular} alt={photo.alt_description} class="image" />
				<p>{photo.likes}</p>
				<div class="info">
					<img src={photo.user.profile_image.medium} alt={photo.user.name} class="profil" />
					<p class="name">{photo.user.name}</p>
				</div>
			</div>
		{/each}
	</div>
</div>

<style>
	.image {
        border-radius: 16px;
	}
	.photos {
        max-width: 100%;
        margin: 0 auto;
        column-count: 3;
        column-gap: 15px;
	}

    .img-container {
        display: block;
		background: red;
        width: 100%;
        height: auto;
        margin-bottom: 15px;
        transition: .2s ease-in-out;
        border-radius: 16px;
	}

    .img-container:hover {
      transform: scale(.95);
      filter: grayscale(100%);
	}

	.container {
		width: 90%;
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
	
    @media (max-width: 757px) {
        .photos {
            column-count: 2;
        }
    }
</style>
