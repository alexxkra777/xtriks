<script>
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';
    import axios from 'axios';
    import Client from "../../components/Users.svelte";

    let storeEmail = null;
    let userID = "";
    let postData = null;
    let loading = true;

    onMount(async () => {
        const storedData = localStorage.getItem('email');
        storeEmail = storedData ? JSON.parse(storedData) : null;

        if (storeEmail == "" || storeEmail == null) {
            goto('../login');
        }

        try {
            const response = await axios.post('https://xtriks.com/api/authorization_client/function.php', { storeEmail });
            userID = response.data[0].user_id;
            if (response.data == "error") {
                goto('../login')
            }
        } catch (error) {
            console.error('Error:', error);
        }

        await getPosts();
    });

    const getPosts = async () => {
        try {
            const response = await axios.post('https://xtriks.com/api/users/function.php', { userID });
            postData = response.data;
            loading = false;
        } catch (error) {
            console.error('Error:', error);
        }
    }

    function signOut(key) {
        localStorage.removeItem(key);
        goto("../")   
    }
</script>

<div class="menu">
    <nav>
        <ul>
            <li>
                <a>
                    <button class="icon" on:click={() => signOut('email')}>
                        Odhlásit se
                    </button>
                </a>
            </li>
        </ul>
    </nav>
</div>

{#if loading}
    <p>Loading...</p>
{:else}
    {#each postData as item}
        <Client name={item.name} surname={item.surname} bussines={item.bussines} email={item.email} adress={item.adress} city={item.city}></Client>
    {/each}
{/if}

<style>
    * {
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

    nav ul {
        list-style-type: none;
        margin: 0;
        padding: 0;
        background-color: #e5d5d1;
        overflow: hidden;
    }

    nav li {
        float: right;
    }

    nav li a {
        display: block;
        text-align: center;
        padding: 14px 16px;
        text-decoration: none;
    }

    button {
        background: none;
        border: none;
        outline: none;
        box-shadow: none;
        font-weight: bold;
        font-size: 20px;
        color: #83464F;
    }
    @media screen and (max-width: 600px) {
        .flexbox {
            margin-left: 0px;
            margin-right: 0px;
            gap: 50px;
        }
    }
</style>
