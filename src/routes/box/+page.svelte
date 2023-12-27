<script>
    import Select from 'svelte-select';
    import Swal from 'sweetalert2'
    import { onMount } from 'svelte';
    import miss_card from '$lib/images/miss_card.jpg';
    //export let data

    // let items = data.boxths.map((item) => `${item.Box_Id} : ${item.Box_Name}` );
    let boxths = [];
    let select2_boxths = [];
    let cardItem = [];

    onMount(async () => {
		const response_boxth = await axios.get('https://api.arymiku.com/YGO/Select/Select_BOXTh.php');
		// photos = await res.json();
        boxths = response_boxth.data;
        select2_boxths = boxths.map(item => {
        return {
            value : item.Id,
            label : `${item.Box_Id} : ${item.Box_Name}`,
            img : item.Box_Pic
        }
        });
	});

    let value_boxth = null;

    async function changeBoxTH(itemSelect){
        value_boxth = itemSelect.detail;
        Swal.showLoading();
        let results = await axios.get(`https://api.arymiku.com/YGO/Select/Select_BoxAllCard.php?Box_Id=${value_boxth.value}`)
        cardItem = results.data;
        Swal.close()
    }

    function showCard(card){
        Swal.fire({
            text : card.Name,
            imageUrl: card.Pic == '' ? miss_card : card.Pic,
            imageAlt: "A tall image"
        });
    }
</script>

<!-- DOC -->
<!-- SVELTE SELECT : https://svelte-select-examples.vercel.app/examples/advanced/style-props -->

<div class="container py-3 mb-4">

    <div class="pt-3 pb-3">
        BOX INFO
    </div>

    <div class="d-flex flex-row">
        <div class="input-group-text">BOX</div>
        <Select items={select2_boxths} on:change={changeBoxTH} />
    </div>

    <div class="row mt-2">
        <div class="col-3 border">
            {#if value_boxth != null}
                <img src="{value_boxth.img}" class="img-fluid" alt="pic_box">
            {/if}
        </div>
        <div class="col-9 border" >
            {#each cardItem as card}
                <input type="image" on:click={() => showCard(card)} src="{card.Pic == '' ? miss_card : card.Pic}" class="img-fluid p-1" style="width: 10%;" alt="card_name" title="{card.Name}" on:error="{() => card.Pic = miss_card}"/>
                
            {/each}
        </div>
    </div>

    <!-- {JSON.stringify(value_boxth)} -->

</div>
