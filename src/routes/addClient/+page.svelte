<script>
  import axios from "axios";
  import { goto } from '$app/navigation';
  import { onMount } from 'svelte';
  import InputField from '../../components/InputField.svelte';
  import BackArrow from "../../components/BackArrow.svelte";
  import Modal from '../../components/ErrorWindow.svelte';

  let storeEmail = null;
  let userId;

  onMount(async () => {
      const storedData = localStorage.getItem('email');
      storeEmail = storedData ? JSON.parse(storedData) : null;

      try {
          const response = await axios.post('https://xtriks.com/api/authorization/function.php', { storeEmail });
          console.log(response.data);
          response.data.forEach(item => {
            userId = item.id; 
          });
          if(response.data == "error"){
              goto('../login')
          }
      } catch (error) {
          console.error('Error:', error);
      }
  });

  let showModal = false;
  let ModalWindow = "";

  let name = '';
  let surname = '';
  let birhday = '';
  let sex = '';
  let allergens = '';
  let chronic = '';
  let pregancy = '';
  let ontology = '';
  let tel = '';
  let email = '';
  let password = '';
  let viralDiseases = '';
  let notice = '';

  const sendData = async () => {
    try {
    const response = await axios.post('https://corsproxy.io/?https://xtriks.com/api/insert_data_clients.php', {userId, name, surname, birhday, sex, allergens, chronic, pregancy, ontology, tel, email, password, viralDiseases, notice})
    if(response.data == "Data inserted successfully"){
      goto('../clients')
    }else{
      showModal = true;
      ModalWindow = "Uživatel s takovým emailem již existuje";
    }
    console.log(response.data);
    } catch (error) {
      console.log(error);
    }
  };
</script>

<Modal bind:showModal>
  <h2 slot="header">{ ModalWindow }</h2>
</Modal>


<BackArrow href="../clients"></BackArrow>
<main>
  <form class="form-container" on:submit|preventDefault={sendData}>
    <div class="flexbox">
      <div class="input-group">
        <InputField label={'Jméno'} bind:value={name}/>
      </div>
      <div class="input-group">
        <InputField label={'Příjmení'} bind:value={surname}/>
      </div>
      <div class="input-group">
        <InputField type={'email'} label={'Email'} bind:value={email}/>
      </div>
      <div class="input-group">
        <InputField type={'password'} label={'Password'} bind:value={password}/>
      </div>
      <div class="input-group">
        <InputField type={'tel'} label={'Telefon'} bind:value={tel}/>
      </div>
      <div class="input-group">
        <InputField label={'Pohlaví'} bind:value={sex}/>
      </div>
      <div class="input-group">
        <InputField type={'date'} label={'Narození'} bind:value={birhday}/>
      </div>
      <div class="input-group">
        <InputField label={'Alergeny'} bind:value={allergens}/>
      </div>
      <div class="input-group">
        <InputField label={'Chronické nemoce'} bind:value={chronic}/>
      </div>
      <div class="input-group">
        <InputField label={'Těhotenství'} bind:value={pregancy}/>
      </div>
      <div class="input-group">
        <InputField label={'Ontologie'} bind:value={ontology}/>
      </div>
      <div class="input-group">
        <InputField label={'Aktivní virusní nemoce'} bind:value={viralDiseases}/>
      </div>
      <div class="input-group textareaGroup">
        <label class="label" for="notice">Poznámky:</label>
        <textarea class="textarea" bind:value={notice}/>
      </div>
    </div>
    <button class="btn submit">Odeslat</button>
  </form>
</main>

<style>
  main {
    font-family: 'Muli', sans-serif;
    display: flex;
    align-items: center;
    justify-content: center;
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
    width: 600px; 
  }

  .flexbox {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
  }

  .input-group {
    flex: 0 0 100%; 
    margin-bottom: 30px; 
  }
  .textareaGroup{
    flex: 100% !important;
  }

  .btn {
    color: white;
    padding: 0.5rem 0;
    margin-top: 0.5rem;
    display: inline-block;
    width: 100%;
    border-radius: 0.25rem;
    cursor: pointer;
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

  .label{
     color: #83464F;
     text-align: left;
  }
  .textarea {
    width: 100%;
    display: block;
    margin-top: 1rem;
    padding: 2rem 0;
    border: 2px solid #83464F;
    border-radius: 10px;
    color: #83464F;
    background-color: #F7E8E9;
    box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
  }
  textarea:focus {
    outline: none;
    border-color: white;
    box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.2);
  }
  
  @media only screen and (min-width: 600px) {
    .input-group {
      flex: 0 0 45%; 
    }
  }
</style>
