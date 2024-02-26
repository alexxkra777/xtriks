<script>
    import { writable } from 'svelte/store';
    import Sidebar from "../../components/menu/Sidebar.svelte";
    import Client from "../../components/client.svelte";
    import AddClient from "../../components/AddClient.svelte";
    import { onMount } from "svelte";
    import { goto } from '$app/navigation';
    import axios from 'axios';

    let storeEmail = null;
    let user_id;
    let list = writable([]);

    onMount(async () => {
        const storedData = localStorage.getItem('email');
        storeEmail = storedData ? JSON.parse(storedData) : null;

        try {
            const response = await axios.post('https://xtriks.com/api/authorization/function.php', { storeEmail });
            if (Array.isArray(response.data) && response.data.length > 0) {
                user_id = response.data[0].id; 
            } else {
                goto('../login');
            }
        } catch (error) {
            console.error('Error:', error);
        }
        await getPosts(); 
    });

    const getPosts = async () => {
        try {
            const response = await axios.post('https://xtriks.com/api/clients/function.php', { user_id });
            list.set(response.data);
        } catch (error) {
            console.error('Error:', error);
        }
    }
</script>

<Sidebar></Sidebar>

<div class="flexbox">
    {#if $list.length > 0}
        {#each $list as item (item.id)}
            <Client name={item.name} surname={item.surname} birhday={item.birhday} email={item.email} tel={item.tel} sex={item.sex} allergens={item.allergens} chronic={item.chronic} pregancy={item.pregancy} ontology={item.ontology} viralDiseases={item.viralDiseases} notice={item.notice} id={item.id}></Client>
        {/each}
    {:else}
        <p>Loading...</p>
    {/if}
    <AddClient></AddClient>
</div>

<style>
    *{
        font-family: sans-serif;
    }
    .flexbox {
        display: flex;
        flex-wrap: wrap;
        margin-left: 400px;
        gap: 75px;
        margin-right: 20px;
        padding-top: 50px;
    }

    @media screen and (max-width: 700px) {
        .flexbox {
            margin-left: 1em;
            margin-right: 0px;
            gap: 50px;
        }
    }
</style>
