<script>
	import { onMount } from 'svelte';

	let log = []

	onMount(async () => {
		const res_log = await axios.get('https://api.arymiku.com/YGO/Select/Select_Log.php');
		log = res_log.data;
	});
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<span class="welcome">
		<!-- <picture>
			<img src={home_wallpaper} alt="Welcome" />
		</picture> -->
		<div class="heading">
			รายละเอียดการเปลี่ยนแปลงต่างๆ
		</div>
		
		<table id="log" >
			<tr class="p-1">
				<td class="p-1">Description</td>
				<td class="p-1">TimeStamps</td>
			</tr>
			{#each log as item}
				<tr class="p-1">
					<td class="p-1">{item.Description}</td>
					<td class="p-1">{item.Timestamp}</td>
				</tr>
			{/each}
		</table>
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

	#log {
		width: 100%;
		height: 100%;
		table-layout: fixed;
		position: relative;
		z-index: 1; /* Ensure content is above the background */
	}

	/* Add background with opacity to the table using ::before */
	#log::before {
		content: '';
		position: absolute;
		width: 100%;
		height: 100%;
		background-image: url(../lib/images/home_wallpaper.jpg);
		background-size: cover;
		opacity: 0.13; /* Adjust opacity for the background */
		z-index: -1; /* Make sure background is behind table content */
	}

	/* Styling table rows */
	.p-1 {
		padding: 10px;
		border: 1px solid black;
	}
	
	.heading {
		margin-bottom: 2%;
		text-align: center;
	}
</style>
