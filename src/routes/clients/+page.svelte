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
            console.log(response.data);
            if (Array.isArray(response.data) && response.data.length > 0) {
                user_id = response.data[0].id; // Assuming you only need the first user_id
            } else {
                goto('../login');
            }
        } catch (error) {
            console.error('Error:', error);
        }
        console.log(storeEmail);
        await getPosts(); // Fetch posts after user_id is set
    });

    const getPosts = async () => {
        try {
            const response = await axios.post('https://xtriks.com/api/clients/function.php', { user_id });
            console.log(response.data);
            list.set(response.data);
            console.log(list); // Assuming your PHP file responds with some data
        } catch (error) {
            console.error('Error:', error);
        }
    }
</script>

<Sidebar></Sidebar>

<div class="flexbox">
    {#if $list.length > 0}
        {#each $list as item (item.id)}
            <Client name={item.name}></Client>
        {/each}
    {:else}
        <p>Loading...</p>
    {/if}
    <AddClient></AddClient>
</div>

<style>
    .flexbox {
        display: flex;
        flex-wrap: wrap;
        margin-left: 400px;
        gap: 75px;
        margin-right: 5px;
        padding-top: 50px;
    }

    @media screen and (max-width: 600px) {
        .flexbox {
            margin-left: 0px;
            margin-right: 0px;
            gap: 50px;
        }
    }
</style>
