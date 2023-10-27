<script lang="ts">
	import { disableScrollHandling } from '$app/navigation';
	import heart from '../lib/heart.svg';

	import axios from 'axios';
	import { onMount } from 'svelte';
	import { fly, fade } from 'svelte/transition';

	let term = '';
	let photos: {
		id: string;
		alt_description: string;
		urls: {
			regular: string;
			full: string;
		};
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
				<p class="like"><img src={heart} alt="heart icon" /> {photo.likes}</p>
				<div class="info">
					<img src={photo.user.profile_image.medium} alt={photo.user.name} class="profil" />
					<p class="name">{photo.user.name}</p>
				</div>
			</div>
            <div class="big-img">
                <img src={photo.urls.regular} alt={photo.urls.regular}>
            </div>
		{/each}
	</div>
</div>

<style>
    .big-img {
        display: none;
        width: 25%;
        position: fixed;
        top: 15%;
        left: 50%;
        transform: translateX(-50%);
        z-index: 1000;
    }

	.container {
		width: 90%;
		margin: 0 auto;
		padding-top: 50px;
	}

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
		width: 100%;
		height: auto;
		margin-bottom: 15px;
		transition: 0.2s ease-in-out;
		border-radius: 16px;
		filter: grayscale(100%);

		position: relative;
	}

	.img-container:hover {
		transform: scale(0.95);
		filter: none;
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
		background-color: #000;
		border-radius: 10px;
		border: none;
		color: white;
		border: 1px solid #000;
		transition: all 0.2s ease-in-out;
	}

	.button:hover {
		background: #fff;
		color: #000;
		border: 1px solid #000;
	}
	.input-container {
		margin: 80px;
	}

	.info {
		display: flex;
		align-items: center;
		position: absolute;
		bottom: 0px;
		padding-left: 25px;
		padding: 5px 0 5px 25px;
		background-color: rgba(255, 255, 255, 0.541);
		width: 100%;

		border-bottom-left-radius: 18px;
		border-bottom-right-radius: 18px;
	}

	.like {
		display: flex;
		position: absolute;
		top: 25px;
		right: 25px;

		background: #fff;
		padding: 5px 10px;
		border-radius: 8px;
		border: solid 1px red;
	}

	.like img {
		padding-right: 5px;
	}
	.profil {
		border-radius: 50%;
		border: solid 2px grey;
		margin-right: 15px;
	}

	@media (max-width: 757px) {
		.photos {
			column-count: 2;
		}
	}
</style>
