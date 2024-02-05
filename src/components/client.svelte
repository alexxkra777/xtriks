<script>
    import Modal from './ModalWindow.svelte';

    export let name;
    export let age;


    let showModal = false;
</script>


<button on:click={() => (showModal = true)}>
    <div class="client">
        <p>{name}<p>
        <p>{age}<p>
    </div>
</button>


<Modal bind:showModal>
	<h2 slot="header">
        {name}
		{age}
	</h2>
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


</style>


