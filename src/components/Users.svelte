<script>
    import Modal from './UserModalWindow.svelte';
    import trash from "$lib/gallery/trash.png";
    import axios from 'axios';

    export let name;
    export let surname;
    export let email;
    export let bussines;
    export let adress;
    export let city;


    let showModal = false;


    
</script>


<button on:click={() => (showModal = true)}>
    <div class="client">
        <h2>{name}<br>{surname}<br>{bussines}<h2>
    </div>
</button>


<Modal bind:showModal>
	<h2 slot="header">
        {name} {surname}
	</h2>
    <h2 slot="email">{email}</h2>
    <h2 slot="bussines">{bussines}</h2>
    <h2 slot="adress">{adress}</h2>
    <h2 slot="city">{city}</h2>
</Modal>

<style>
    .client{
        height: 350px;
        width: 250px;
        background-color: #e5d5d1;
        border: 3px solid #83464F;
        border-radius: 25px;
        margin-top: 10%;
        margin-left: 25%;
    }

    button {
	background: none;
	color: inherit;
	border: none;
	padding: 0;
	font: inherit;
	cursor: pointer;
	outline: inherit;
    }
    h2{
        color: #83464F;
        font-size: 22px;
    }
    @media screen and (max-width: 700px) {
        h2{
            font-size: 17px;
        }
    }


</style>


