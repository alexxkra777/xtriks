<script>
    import icon from "$lib/gallery/icon.png"
    import { goto } from '$app/navigation';
    import Modal from './AccountWindow.svelte'
    import axios from 'axios';
    import { onMount } from 'svelte';

    let storeEmail = null;
    let name = "";

    onMount(async () => {
        const storedData = localStorage.getItem('email');
        storeEmail = storedData ? JSON.parse(storedData) : null;

        if(storeEmail == "" || storeEmail == null){
            goto('../login');
        }

        try {
            const response = await axios.post('https://xtriks.com/api/authorization/function.php', { storeEmail });
            //console.log(response.data);
            response.data.forEach(item => {
                name = item.name;
            });
        } catch (error) {
            console.error('Error:', error);
        }
    });



    let showModal = false;

    function signOut(key) {
        localStorage.removeItem(key);
        goto("../")   
    }

    function open(){
        showModal = true;
    }
</script>
    <button class="icon" on:click={() => open()}>
        <img src={icon} alt="icon">
    </button>    


    <Modal bind:showModal>
        <h2 slot="header" class="name">{name}</h2>
        <button slot="button" class="icon" on:click={() => signOut('email')}>
            Odhlásit se
        </button>   
    </Modal>
<style>
    *{
        font-family: sans-serif;
    }
    button {
        border: none;
        background: none;
        padding: 0;
        margin: 0;
        cursor: pointer;

    }
    .name{
        color: #83464F;
    }
    .icon{
        height: 2em;
        width: 6em;
        font-weight: bold;
        color: #83464F;
        font-size: 20px;
    }
    img{
        height: 45px;
    }
</style>