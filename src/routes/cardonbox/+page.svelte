<script>
    import Select from 'svelte-select';
    import Swal from 'sweetalert2'
    import { onMount } from 'svelte';
    import miss_card from '$lib/images/miss_card.jpg';
    //export let data

    // let items = data.boxths.map((item) => `${item.Box_Id} : ${item.Box_Name}` );
    let cards = [];
    let select2_cards = []
    let cardItem = [];

    let disabled_select2 = false;

    onMount(async () => {
        const response_cardlist = await axios.get('https://api.arymiku.com/YGO/Select/Select_Card.php');
        cards = response_cardlist.data;
        select2_cards = cards.map(item => {
            return {
                value : item.Id,
                label : `${item.Card_Number} : ${item.Name}`,
                cardNumber : item.Card_Number,
                img : item.Pic
            }
        })
	});

    let value_cardNumber = null;

    async function changeBoxTH(itemSelect){
        value_cardNumber = itemSelect.detail.map(item => item.cardNumber).toString();
        disabled_select2 = true;
        let results = await axios.get(`https://api.arymiku.com/YGO/Select/Select_Search_CardBoxTH_V2.php?CardNumber_List=${value_cardNumber}`);
        cardItem = results.data;
        disabled_select2 = false;
    }

    function SearchData(card){
        Swal.fire({
            text : card.Name,
            imageUrl: card.Pic == '' ? miss_card : card.Pic,
            imageAlt: "A tall image"
        });
    }

    function showBox(box){
        Swal.fire({
            text : box.BoxName,
            imageUrl: box.Pic == '' ? miss_card : box.BoxPic,
            imageAlt: "A tall image"
        });
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
        CARD INFO
    </div>

    <div class="d-flex flex-row">
        <div class="input-group-text">CARD</div>
        <Select items={select2_cards} on:change={changeBoxTH} multiple disabled={disabled_select2}/>
    </div>

    <div class="row mt-2">
        {#each cardItem as card}
            <div class="col-3 p-1 border">
                <input type="image" src="{card.Pic == '' ? miss_card : card.Pic}" on:click={() => showCard(card)} class="img-fluid" alt="pic_box">
            </div>
            <div class="col-9 p-1 border" >
                {#each card.BoxList as box}
                    <input type="image" src="{box.BoxPic == '' ? miss_card : box.BoxPic}" on:click={() => showBox(box)} class="img-fluid p-1" style="width: 20%;" alt="card_name" title="{box.BoxName}" on:error="{() => card.Pic = miss_card}"/>
                    
                {/each}
            </div>
        {/each}
    </div>

    <div class="d-flex justify-content-center mt-3">
        <button type="button" class="btn btn-outline-success" on:click={SearchData}>SEARCH</button>
    </div>

</div>
