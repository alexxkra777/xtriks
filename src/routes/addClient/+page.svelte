<script>
  import axios from "axios";
  import { goto } from '$app/navigation';
  import { onMount } from 'svelte';
  import InputField from '../../components/InputField.svelte';
  import BackArrow from "../../components/BackArrow.svelte";

  let storeEmail = null;
  let userId;

  onMount(async () => {
      const storedData = localStorage.getItem('email');
      storeEmail = storedData ? JSON.parse(storedData) : null;

      try {
          const response = await axios.post('https://xtriks.com/api/authorization/function.php', { storeEmail });
          console.log(response.data);
          response.data.forEach(item => {
            userId = item.id; // You can write the data to the console or use it as needed
          });
          if(response.data == "error"){
              goto('../login')
          }
      } catch (error) {
          console.error('Error:', error);
          // Redirect or handle error appropriately
      }
  });

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
    goto('/clients')
    console.log(response.data);
    } catch (error) {
      console.log(error);
    }
  };
</script>

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
        <InputField type={'telephone'} label={'Telefon'} bind:value={tel}/>
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
      <div class="input-group">
        <label>Poznámky</label>
        <textarea bind:value={notice}/>
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
    width: 600px; /* Adjusted width to accommodate two input fields per row */
  }

  .flexbox {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
  }

  .input-group {
    flex: 0 0 100%; /* Full width on small screens */
    margin-bottom: 30px; /* Increased margin between input groups */
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

  .submit {
    background: linear-gradient(to bottom, #44c767 5%, #50b01c 100%);
    background-color: #44c767;
  }

  .submit:hover {
    background: linear-gradient(to bottom, #50b01c 5%, #44c767 100%);
    background-color: #50b01c;
  }

  .message {
    text-align: center;
  }

  @media only screen and (min-width: 600px) {
    /* Adjust styles for screens wider than 600px */
    .input-group {
      flex: 0 0 45%; /* Adjusted width to accommodate two input fields per row with some spacing */
    }
  }
</style>
