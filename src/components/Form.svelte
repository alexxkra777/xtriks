<script>
	import InputField from './InputField.svelte';
    import axios from 'axios';
    import { goto } from '$app/navigation';
    import Modal from './ErrorWindow.svelte';
	export let active_step;


    let name = "";
    let surname = ""; 
    let email = ""; 
    let password = ""; 
    let bussines = "";
    let adress = ""; 
    let city = "";

    let showModal = false;
    let ModalWindow = "";
	
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
                    ModalWindow = "Chyba v registraci, email už existuje";
                } else {   
                    if(response.data.status === 'error'){
                        console.log("error with inserting data");
                        ModalWindow = "Chyba v přihlášení, špatná data";
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


<Modal bind:showModal>
    <h2 slot="header">
        { ModalWindow }
	</h2>
</Modal>

<form class="form-container" on:submit|preventDefault={handleSubmit}>
	{#if active_step == 'Info'}
		<InputField label={'Jméno'} bind:value={name}/>
		<InputField label={'Příjmení'} bind:value={surname}/>
		<InputField type={'email'} label={'Email'} bind:value={email}/>
		<InputField type={'password'} label={'Heslo'} bind:value={password}/>
        <InputField label={'Podnikání'} bind:value={bussines}/>
	{:else if active_step == 'Adresa'}
		<InputField label={'Adresa'} bind:value={adress}/>
		<InputField label={'Město'} bind:value={city}/>
	{:else if active_step == 'Registrace'}
		<div class="message">
			<h2>Děkujeme za data</h2>
			<button class="btn submit">Zaregistrovat se</button>
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