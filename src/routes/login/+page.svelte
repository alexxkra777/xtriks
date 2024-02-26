<script>
    import InputField from '../../components/InputField.svelte';
    import BackArrow from "../../components/BackArrow.svelte";
    import axios from 'axios';
    import { goto } from '$app/navigation';
    import Modal from '../../components/ErrorWindow.svelte';
    import icon from "$lib/gallery/angle-circle-left.png";
    import crm from "$lib/gallery/crm.png";
    import following from "$lib/gallery/following.png"

    let email = "";
    let password = "";
    let emailClient = "";
    let passwordClient = "";
    let showModal = false;
    let login = false; 
    let ModalWindow = "";
    let showUserLogin = false;
    let showClientLogin = false;
    let buttonsVisible = true;

    const handleSubmit = async () => {
        function addToSessionStorage(key, value) {
            localStorage.setItem(key, JSON.stringify(value));
        }

        try {
            const response = await axios.post('https://xtriks.com/api/login/function.php', { email, password });
            if (response.data == 'badlogin') {
                showModal = true;
                login = false;
                ModalWindow = "Chyba v přihlášení, špatné heslo nebo e-mail";
            } else {
                login = true;
            }
        } catch (error) {
            console.error('Error:', error);
        }
        if (login == true) {
            addToSessionStorage('email', email);
            goto('/calendar');
        }
    }

    const handleSubmitClient = async () => {
        function addToSessionStorage(key, value) {
            localStorage.setItem(key, JSON.stringify(value));
        }

        try {
            const response = await axios.post('https://xtriks.com/api/login_clients/function.php', { emailClient, passwordClient });
            if (response.data == 'badlogin') {
                showModal = true;
                login = false;
                ModalWindow = "Chyba v přihlášení, špatné heslo nebo e-mail";
            } else {
                login = true;
            }
        } catch (error) {
            console.error('Error:', error);
        }
        if (login == true) {
            addToSessionStorage('email', emailClient);
            goto('/forClients');
        }
    }

    const handleUserButtonClick = () => {
        showUserLogin = true;
        showClientLogin = false;
        buttonsVisible = false;
    }

    const handleClientButtonClick = () => {
        showClientLogin = true;
        showUserLogin = false;
        buttonsVisible = false;
    }

    const handleBackButtonClick = () => {
        showUserLogin = false;
        showClientLogin = false;
        buttonsVisible = true;
    }
</script>

<Modal bind:showModal>
    <h2 slot="header">{ ModalWindow }</h2>
</Modal>

<BackArrow href="../../"></BackArrow>
<main>
    <div class="choise" style="{buttonsVisible ? '' : 'display: none;'}">
        <div on:click={handleUserButtonClick} class="choise-button">
            <img src={crm} alt="crm">
            <h2>Přihlásit se do mého kabinetu</h2>
        </div>
        <div on:click={handleClientButtonClick} class="choise-button">
            <img src={following} alt="client">
            <h2>Přihlásit se jako klient</h2>
        </div>
    </div>

    {#if showUserLogin}
        <form class="form-container login_user" on:submit|preventDefault={handleSubmit}>
            <div on:click={handleBackButtonClick} class="btn back"><img src={icon} alt="back"></div>
            <InputField label={'Email'} bind:value={email}/>
            <InputField type={'password'} label={'Heslo'} bind:value={password}/>
            <button class="btn submit">Přihlásit se</button>
        </form>
    {/if}

    {#if showClientLogin}
        <form class="form-container login_client" on:submit|preventDefault={handleSubmitClient}>
            <div on:click={handleBackButtonClick} class="btn back"><img src={icon} alt="back"></div>
            <InputField label={'Email'} bind:value={emailClient}/>
            <InputField type={'password'} label={'Heslo'} bind:value={passwordClient}/>
            <button class="btn submit">Přihlásit se</button>
        </form>
    {/if}
</main>


<style>
    *{
        font-family: sans-serif;
    }
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
    .choise-button {
        display: block;
        border: none;
        margin: 0;
        padding: 0;
        font: inherit;
        border-radius: 0;
        appearance: none; 
        background-color: #e5d5d1;
        border-radius: 10px;
        height: 10em;
        width: 30em;
        text-align: center;
        cursor: pointer;
    }
    .choise-button h2{
        color: #83464F;
        font-weight: bold;
        transition: 200ms;
    }
    .choise-button img{
        margin-top: 2em;
        height: 50px;
        width: 50px;
    }
    .choise{
        display: flex;
        flex-direction: row;
        gap: 10px;
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
    .back{
        font-size: 14px;
        text-align: left;
        text-decoration: line;
    }
	.submit{
		background:linear-gradient(to bottom, #83464F 5%, #83464F 100%);
		background-color: #83464F;
        border-color: #e5d5d1;
        color: #e5d5d1;
        border-radius: 10px;
        
	}
	.submit:hover {
		background:linear-gradient(to bottom, #83464F 5%, #83464F 100%);
		background-color:#83464F;
	}
    img{
        height: 25px;
        width: 25px;
        margin-right: 1%
    }
    @media screen and (max-width: 700px) {
        .choise{
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
    }
</style>
