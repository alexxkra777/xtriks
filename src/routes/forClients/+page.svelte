<script>
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';

    let storeEmail = null;

    onMount(async () => {
        const storedData = localStorage.getItem('email');
        storeEmail = storedData ? JSON.parse(storedData) : null;

        if(storeEmail == "" || storeEmail == null){
            goto('../login');
        }

        try {
            const response = await axios.post('https://xtriks.com/api/authorization_client/function.php', { storeEmail });
            console.log(response.data);
            response.data.forEach(item => {
                user_id = item.id; // You can write the data to the console or use it as needed
            });
            if(response.data == "error"){
                goto('../login')
            }
        } catch (error) {
            console.error('Error:', error);
            // Redirect or handle error appropriately
        }
    });
</script>