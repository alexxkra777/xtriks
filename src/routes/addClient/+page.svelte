<script>
  import axios from "axios";
  import { goto } from '$app/navigation';

  let name = '';
  let age = '';
  let email = '';

  const sendData = async () => {
    try {
    const response = await axios.post('http://xtriks.local/BACK/insert_data.php', {name, age, email})
    goto('/clients')
    } catch (error) {
      console.log(error);
    }
    
  };
</script>

<main>
  <h1>Insert Data</h1>
  <form on:submit|preventDefault={sendData}>
    <label>
      Name:
      <input bind:value={name} />
    </label>
    <label>
      Age:
      <input type="number" bind:value={age} />
    </label>
    <label>
      email:
      <input type="email" bind:value={email} />
    </label>
    <button type="submit">Insert Data</button>
  </form>
</main>
