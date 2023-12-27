<script>
	import home_wallpaper from '$lib/images/home_wallpaper.jpg'
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<span class="welcome">
		<picture>
			<img src={home_wallpaper} alt="Welcome" />
		</picture>
	</span>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	.welcome {
		display: block;
		position: relative;
	}

	
</style>
