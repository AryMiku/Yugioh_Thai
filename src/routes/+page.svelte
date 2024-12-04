<script>
	import { onMount } from 'svelte';

	let log = []
	let box_log_detail = {
		month : [],
		day : [],
		all : []
	}
	let card_log_detail = {
		month : [],
		day : [],
		all : []
	}

	onMount(async () => {
		const [res_log, res_log_box] = await Promise.all([
			axios.get('https://api.arymiku.com/YGO/Select/Select_Log.php'),
			axios.get('https://api.arymiku.com/YGO/Select/Select_Log_CardBox.php')
		]);

		log = res_log.data;

		["month", "day", "all"].forEach((group) => {
    		box_log_detail[group] = res_log_box.data.box.filter(item => item.grouping === group);
		});

		["month", "day", "all"].forEach((group) => {
    		card_log_detail[group] = res_log_box.data.card.filter(item => item.grouping === group);
		});
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

	<div class="container mt-3">
		<div class="row">
			<div class="col-12 col-md-6">
				<ul class="nav nav-tabs" id="myTab" role="tablist">
					<li class="nav-item" role="presentation">
					  <button class="nav-link active" id="box-log-today-tab" data-bs-toggle="tab" data-bs-target="#boxlogtoday" type="button" role="tab" aria-controls="boxlogtoday" aria-selected="true">Box นิยมวันนี้</button>
					</li>
					<li class="nav-item" role="presentation">
					  <button class="nav-link" id="box-log-month-tab" data-bs-toggle="tab" data-bs-target="#boxlogmonth" type="button" role="tab" aria-controls="boxlogmonth" aria-selected="false">Box นิยมเดือนนี้</button>
					</li>
					<li class="nav-item" role="presentation">
					  <button class="nav-link" id="box-log-all-tab" data-bs-toggle="tab" data-bs-target="#boxlogall" type="button" role="tab" aria-controls="boxlogall" aria-selected="false">Box นิยมทั้งหมด</button>
					</li>
				</ul>
				<div class="tab-content" id="myTabContent">
				<div class="tab-pane fade show active" id="boxlogtoday" role="tabpanel" aria-labelledby="box-log-today-tab">
					{#if box_log_detail.day.length == 0}
						<p>วันนี้ยังไม่มีใครค้นหาอะไรเลย T.T</p>
					{:else}
						{#each box_log_detail.day as bm}
							<div class="image-wrapper me-2">
								<img src="{bm.Box_Pic}" alt="{bm.Box_Name}" class="img-fluid" width="130" height="150">
								<span class="badge-count">View {bm.seen}</span>
							</div>
						{/each}
					{/if}
					
				</div>
				<div class="tab-pane fade" id="boxlogmonth" role="tabpanel" aria-labelledby="box-log-month-tab">
					{#if box_log_detail.day.length == 0}
						<p>วันนี้ยังไม่มีใครค้นหาอะไรเลย T.T</p>
					{:else}
						{#each box_log_detail.month as bm}
							<div class="image-wrapper me-2">
								<img src="{bm.Box_Pic}" alt="{bm.Box_Name}" class="img-fluid" width="130" height="150">
								<span class="badge-count">View {bm.seen}</span>
							</div>
						{/each}
					{/if}
				</div>
				<div class="tab-pane fade" id="boxlogall" role="tabpanel" aria-labelledby="box-log-all-tab">
					{#if box_log_detail.day.length == 0}
						<p>วันนี้ยังไม่มีใครค้นหาอะไรเลย T.T</p>
					{:else}
						{#each box_log_detail.all as bm}
							<div class="image-wrapper me-2">
								<img src="{bm.Box_Pic}" alt="{bm.Box_Name}" class="img-fluid" width="130" height="150">
								<span class="badge-count">View {bm.seen}</span>
							</div>
						{/each}
					{/if}
				</div>
				</div>
			</div>
			<div class="col-12 col-md-6">
				<ul class="nav nav-tabs" id="TabCardLog" role="tablist">
					<li class="nav-item" role="presentation">
					  <button class="nav-link active" id="card-log-today-tab" data-bs-toggle="tab" data-bs-target="#cardlogtoday" type="button" role="tab" aria-controls="cardlogtoday" aria-selected="true">Card นิยมวันนี้</button>
					</li>
					<li class="nav-item" role="presentation">
					  <button class="nav-link" id="card-log-month-tab" data-bs-toggle="tab" data-bs-target="#cardlogmonth" type="button" role="tab" aria-controls="cardlogmonth" aria-selected="false">Card นิยมเดือนนี้</button>
					</li>
					<li class="nav-item" role="presentation">
					  <button class="nav-link" id="card-log-all-tab" data-bs-toggle="tab" data-bs-target="#cardlogall" type="button" role="tab" aria-controls="cardlogall" aria-selected="false">Card นิยมทั้งหมด</button>
					</li>
				</ul>
				<div class="tab-content" id="TabCardLogContent">
				<div class="tab-pane fade show active" id="cardlogtoday" role="tabpanel" aria-labelledby="card-log-today-tab">
					{#if box_log_detail.day.length == 0}
						<p>วันนี้ยังไม่มีใครค้นหาอะไรเลย T.T</p>
					{:else}
						{#each card_log_detail.day as bm}
							<div class="image-wrapper me-2">
								<img src="{bm.Box_Pic}" alt="{bm.Box_Name}" class="img-fluid" width="130" height="150">
								<span class="badge-count">View {bm.seen}</span>
							</div>
						{/each}
					{/if}
				</div>
				<div class="tab-pane fade" id="cardlogmonth" role="tabpanel" aria-labelledby="card-log-month-tab">
					{#if box_log_detail.day.length == 0}
						<p>วันนี้ยังไม่มีใครค้นหาอะไรเลย T.T</p>
					{:else}
						{#each card_log_detail.month as bm}
							<div class="image-wrapper me-2">
								<img src="{bm.Box_Pic}" alt="{bm.Box_Name}" class="img-fluid" width="130" height="150">
								<span class="badge-count">View {bm.seen}</span>
							</div>
						{/each}
					{/if}
				</div>
				<div class="tab-pane fade" id="cardlogall" role="tabpanel" aria-labelledby="card-log-all-tab">
					{#if box_log_detail.day.length == 0}
						<p>วันนี้ยังไม่มีใครค้นหาอะไรเลย T.T</p>
					{:else}
						{#each card_log_detail.all as bm}
							<div class="image-wrapper me-2">
								<img src="{bm.Box_Pic}" alt="{bm.Box_Name}" class="img-fluid" width="130" height="150">
								<span class="badge-count">View {bm.seen}</span>
							</div>
						{/each}
					{/if}
				</div>
			</div>
		</div>
	</div>
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

	.image-wrapper {
      position: relative;
      display: inline-block;
    }
    .image-wrapper img {
      border: 2px solid #ccc; /* Optional: Adds a border around the image */
    }
    .badge-count {
      position: absolute;
      top: 5px; /* Adjust for spacing */
      right: 5px; /* Adjust for spacing */
      background-color: red;
      color: white;
      font-size: 0.75rem;
      padding: 0.25em 0.5em;
    }
</style>
