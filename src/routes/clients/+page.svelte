<script>

    import Sidebar from "../../components/menu/Sidebar.svelte";
    import Client from "../../components/client.svelte";
    import Account from "../../components/Account.svelte";
    import AddClient from "../../components/AddClient.svelte";
	import { onMount } from "svelte";

	const getPosts = async () => {
		const res = await fetch("https://xtriks.com/api/customers/read.php");
		const data = await res.json();
		console.log(data);
		return data;
	}
</script>

<Account></Account>

<Sidebar></Sidebar>

<div class="flexbox">
    {#await getPosts()}
	<p>Loading...</p>
    {:then data}
        {#each data as { name, age }}
            <Client name={name} age={age}>
            </Client>
        {/each}
    {/await}
    <AddClient></AddClient>
</div>

<style>
    .flexbox{
        display: flex;
        flex-wrap: wrap;
        margin-left: 400px;
        gap: 75px;
        margin-right: 5px;
        padding-top: 50px;
    }
</style>



