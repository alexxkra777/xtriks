<script>
	import { onMount } from "svelte";

	const getPosts = async () => {
		const res = await fetch("https://xtriks.com/api/customers/read.php");
		const data = await res.json();
		console.log(data);
		return data;
	}
</script>


{#await getPosts()}
	<p>Loading...</p>
{:then data}
	{#each data as { name, age }}
		<div>
			<p>{name}</p>
			<p>{age}</p>
		</div>
	{/each}
{/await}
