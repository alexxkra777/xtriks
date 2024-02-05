<script>
    import { onMount } from 'svelte';
    import Modal from "../../components/ModalWindow.svelte";
  
    let showModal = false;
    let data = [];
  
    // Fetch data from API
    onMount(async () => {
      const response = await fetch('http://xtriks.local/BACK/customer/read.php');
      data = await response.json();
    });
  </script>
  
  <h1>SvelteKit Modal Example</h1>
  
  <button on:click={() => showModal = true}>Open Modal</button>
  
  <!-- Pass data and showModal state to the modal component -->
  <Modal {data} {showModal} />