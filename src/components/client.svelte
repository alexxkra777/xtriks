<script>
    import Modal from './ModalWindow.svelte';
    import trash from "$lib/gallery/trash.png";
    import axios from 'axios';

    export let name;
    export let surname;
    export let birhday;
    export let email;
    export let tel;
    export let sex;
    export let allergens;
    export let chronic;
    export let pregancy;
    export let ontology;
    export let viralDiseases; 
    export let notice;
    export let id;


    let showModal = false;


    
    const handleDeleteEvent = async (id) => {
        try {
            const response = await axios.post('https://xtriks.com/api/delete_data_client.php', { id });
            window.location.reload();
        } catch (error) {
            console.error('Error:', error);
        }
    }
</script>


<button on:click={() => (showModal = true)}>
    <div class="client">
        <h2>{name}<br>{surname}<h2>
    </div>
</button>


<Modal bind:showModal>
	<h2 slot="header">
        {name} {surname}
	</h2>
    <h2 slot="email">{email}</h2>
    <h2 slot="birhday">{birhday}</h2>
    <h2 slot="tel">{tel}</h2>
    <h2 slot="sex">{sex}</h2>
    <h2 slot="allergens">{allergens}</h2>
    <h2 slot="chronic">{chronic}</h2>
    <h2 slot="pregancy">{pregancy}</h2>
    <h2 slot="ontology">{ontology}</h2>
    <h2 slot="viralDiseases">{viralDiseases}</h2>
    <h2 slot="notice">{notice}</h2>
    <img src={trash} alt="delete"  role="button" on:click={handleDeleteEvent({id})} slot="delete" class="dlt">
</Modal>

<style>
    .client{
        height: 250px;
        width: 150px;
        background-color: #e5d5d1;
        border: 3px solid #83464F;
        border-radius: 25px;
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
        font-size: 18px;
    }
    .dlt{
        height: 50px;
        width: 50px;
        cursor: pointer;
    }
    @media screen and (max-width: 700px) {
        h2{
            font-size: 15px;
        }
    }


</style>


