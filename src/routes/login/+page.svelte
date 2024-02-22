<script>
    import InputField from '../../components/InputField.svelte';
    import BackArrow from "../../components/BackArrow.svelte";
    import axios from 'axios';
    import { goto } from '$app/navigation';
    import Modal from '../../components/ModalWindow.svelte';



    let email = "";
    let password = "";
    let showModal = false;
    let login = false; 

    const handleSubmit = async () => {
    //console.log("Your form data => ",formData)
        function addToSessionStorage(key, value) {
            localStorage.setItem(key, JSON.stringify(value));
        }

        try {
            const response = await axios.post('https://xtriks.com/api/login/function.php', { email, password });
            if (response.data == 'badlogin') {
                showModal = true;
                login = false;
            } else {
                login = true;
                console.log(login);
                //console.log("pizda");
                //showModal = true;
            }

            console.log(response.data);
        } catch (error) {
            console.error('Error:', error);
        }
        if(login == true){
            addToSessionStorage('email', email);
            goto('/calendar');
        }
        
    }



</script>

<Modal bind:showModal></Modal>

<BackArrow href="../../"></BackArrow>
<main>
    <form class="form-container" on:submit|preventDefault={handleSubmit}>
        <InputField label={'Email'} bind:value={email}/>
        <InputField type={'password'} label={'Password'} bind:value={password}/>
        <button class="btn submit">Finish </button>
    </form>
</main>

<style>

	main {
		font-family: 'Muli', sans-serif;
		display: flex;
		align-items: center;
		justify-content: center;
		height: 100vh;
		overflow: hidden;
		margin: 0;
	}
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