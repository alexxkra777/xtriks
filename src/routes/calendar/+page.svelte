<script>
    import Calendar from '../../components/Calendar.svelte';
    import Sidebar from "../../components/menu/Sidebar.svelte";
    import axios from 'axios';
    import { writable } from 'svelte/store';
    import Appointment from '../../components/Appointment.svelte';
    import { fade, fly } from 'svelte/transition';
    import { createEventDispatcher } from 'svelte';
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
            const response = await axios.post('https://xtriks.com/api/authorization/function.php', { storeEmail });
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



    
    
    let schedulerShowing = false;    
    let dateID = "";
    let dateHeading = "";

    let eventName = "";
    let hour = "";
    let minutes = "";
    let user_id;
    let client_id = "1";

    let list = writable([]);

    const makeDateHeading = () => {
        let dateAsHeading = dateID.replace(/_/g, " ");
        let date = new Date(`${dateAsHeading}`);
        return dateHeading = date.toLocaleString("en-US", {day: 'numeric', month: 'long', year: 'numeric'} );
    }
    
    const handleScheduler = async (e) => {
        schedulerShowing = true;
        dateID = e.target.dataset.dateid;
        makeDateHeading();
        console.log("open modal");
        console.log(dateID)
		readData()
    }

	const readData = async () => {
		try {
            const response = await axios.post('https://xtriks.com/api/appointment/function.php', {dateID, user_id});
            list.set(response.data);
            console.log(list); // Assuming your PHP file responds with some data
        } catch (error) {
            console.error('Error:', error);
        }
	}
    

    
    const closeScheduler = () => {
        schedulerShowing = false;
    }

    const submitAppt = async () => {
		let date_id = dateID;
        // Send data to PHP file using Axios
        try {
            const response = await axios.post('https://xtriks.com/api/insert_data_calendar.php', { date_id ,eventName, hour, minutes, user_id, client_id });
            console.log(response.data); // Assuming your PHP file responds with some data
			readData();
        } catch (error) {
            console.error('Error:', error);
        }
    };
</script>

<main>
    <Sidebar></Sidebar>
    <div class="calendar">
        <Calendar on:click={handleScheduler} />
        {#if schedulerShowing}
            <section transition:fade={{ duration: 125 }}>
                <form method="post" id={dateID} on:submit|preventDefault={submitAppt}>
                    <div id="closer-cont">
                        <span on:click={() => closeScheduler()} class="close" title="Close Modal">&times;</span>
                    </div>
                    <header>
                        <h2>My Schedule for</h2>
                        <h2>{dateHeading}</h2>
                        <input type="text" id="event-input" required placeholder="Add an event..." bind:value={eventName}>
                        <div id="time-cont">
                            <div id="hrs-mins-cont">
                                <input type="number" id="timeHour-input" name="timeHour" min="1" max="24" step="1" placeholder="Hr." bind:value={hour}>
                                <span id="time-colon">:</span>
                                <input type="number" id="timeMinutes-input" name="timeMinutes" min="0" max="59" step="1" placeholder="Mins." bind:value={minutes}>
                            </div>   
                        </div>
                        <div>
                            <button class="addBtn">Add</button>
                        </div>  
                    </header>
                	{#each $list as item (item.id)}
                        <div>{item.event_name}</div>
                    {/each}
                </form>
            </section>
        {/if}
    </div>
</main> 

<style>
    .calendar{
        margin-left: 350px;
        margin-right: 35px;
    }
    section { 
        box-sizing: border-box;
        width: 100%;
        height: 100vh;
        position: absolute;
        left: 0;
        top: 0;
        background-color: white;
    }
    
    h1 {
        text-align: center;
        color: gray;
    }
    
    h2 {
        margin: 5px 0;
    }
    
    /* Bordered form */
    form {
        top: 0; left: 0; bottom: 0; right: 0;
        z-index: 10;
        overflow: auto;
        margin: auto;
        background-color: white;
        box-shadow: 0 0 10px black;
        
    }
        
    table {
        border-collapse: collapse;
        border-spacing: 0;
        width: 100%;
        border: 1px solid #ddd;
        background-color: white;
    }
    
    /* Style the close button */
    .close {
        position: absolute;
        font-size: 2rem;
        color: white;
        right: 0;
        top: 0;
        padding: 8px 16px 12px 16px;
        cursor: pointer;
    }

    .close:hover {
        background-color: hsl(168, 76%, 40%);
    }

    /* Style the header */
    header {
        display: flex;
        flex-direction: column;
        align-items: center;
        background-color: hsl(33, 92%, 29%);
        padding: 30px 40px;
        color: white;
    }
    
    #time-cont {
        width: 100%;
        display: flex;
        justify-content: center;
        margin-bottom: 10px;
    }
    
    #hrs-mins-cont {
        width: 200px;
        display: flex;
        justify-content: space-between;
    }
    
    #time-colon {
        font-size: 3rem;
    }
    
    input[type=number] {
        margin-right: 5px;
    }
    
    input[type=number]::-webkit-inner-spin-button {
    opacity: 1
    }
    
    #am-pm-cont > div {
        width: 60px;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    
    input[type="radio"] {
        width: 20px;
    }
    
    /* Style the input */
    input {
        margin: 10px 0;
        border: none;
        border-radius: 0;
        width: 300px;
        padding: 10px;
        float: left;
        font-size: 1.1rem;
    }

    /* Style the "Add" button */
    
    .addBtn {
        padding: 10px;
        width: 160%;
        background: hsl(168, 76%, 40%);
        color: #FFF;
        float: left;
        text-align: center;
        font-size: 16px;
        cursor: pointer;
        transition: 0.1s;
        border: 1px solid hsl(168, 76%, 40%);
        border-radius: 0;
    }

    .addBtn:hover {
        border: 1px solid white;
        background-color: hsl(168, 76%, 35%);
    }   

    .addBtn:active {
        background-color: hsl(168, 76%, 100%);
        color: hsl(168, 76%, 25%);
    }   
    @media screen and (max-width: 600px) {
		.calendar{
            margin-left: 0px;
            margin-right: 0px;
		}
	}
</style>
