<script>
	import InputField from './InputField.svelte';
    import axios from 'axios';
    import { goto } from '$app/navigation';
	export let active_step;


    let name = "";
    let surname = ""; 
    let email = ""; 
    let password = ""; 
    let bussines = "";
    let adress = ""; 
    let city = "";
	
	const handleSubmit = async () => {
		//console.log("Your form data => ",formData)

        function addToSessionStorage(key, value) {
            localStorage.setItem(key, JSON.stringify(value));
        }

        try {
            const response = await axios.post('https://corsproxy.io/?https://xtriks.com/api/insert_data_users.php', { name, surname, email, password, bussines, adress, city });
            if (response.data !== null) {
                if (response.data.status === 'error' && response.data.message === 'Email already exists') {
                    console.log('Error: Email already exists');
                } else {   
                    if(response.data.status === 'error'){
                        console.log("error with inserting data");
                    }else{
                        addToSessionStorage('email', email);
                        goto('/calendar');
                    }
                }
            }  
            console.log(response.data);
        } catch (error) {
            console.error('Error:', error);
        }
	}

</script>

<form class="form-container" on:submit|preventDefault={handleSubmit}>
	{#if active_step == 'Info'}
		<InputField label={'Name'} bind:value={name}/>
		<InputField label={'Surname'} bind:value={surname}/>
		<InputField label={'Email'} bind:value={email}/>
		<InputField type={'password'} label={'Password'} bind:value={password}/>
        <InputField label={'Podnikání'} bind:value={bussines}/>
	{:else if active_step == 'Address'}
		<InputField label={'Address'} bind:value={adress}/>
		<InputField label={'City'} bind:value={city}/>
	{:else if active_step == 'Zaregistrováno'}
		<div class="message">
			<h2>Thank you for choosing us</h2>
			<button class="btn submit">Finish </button>
		</div>
	{/if}
</form>

<style>
	
	.form-container {
		background-color: #e5d5d1;
		border-radius: 10px;
		box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1), 0 6px 6px rgba(0, 0, 0, 0.1);
		padding: 50px 20px;
		text-align: center;
		max-width: 100%;
		width: 350px;
	}
	.btn{
		color: white;
		padding: 0.5rem 0;
		margin-top: 0.5rem;
		display: inline-block;
		width: 100%;
		border-radius: 0.25rem;
		cursor:pointer;
	}
	.submit{
		background:linear-gradient(to bottom, #44c767 5%, #50b01c 100%);
		background-color:#44c767;
	}
	.submit:hover {
		background:linear-gradient(to bottom, #50b01c 5%, #44c767 100%);
		background-color:#50b01c;
	}
	.message{
		text-align: center;
	}
</style>