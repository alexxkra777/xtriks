<script>
    import Calendar from '../../components/Calendar.svelte';
    import Sidebar from "../../components/menu/Sidebar.svelte";
    import axios from 'axios';
    import { writable } from 'svelte/store';
    import { fade, fly } from 'svelte/transition';
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';
    import close from "$lib/gallery/close_icon.png";
    import trash from "$lib/gallery/trash.png"

    let storeEmail = null;

    onMount(async () => {
        const storedData = localStorage.getItem('email');
        storeEmail = storedData ? JSON.parse(storedData) : null;

        if(storeEmail == "" || storeEmail == null){
            goto('../login');
        }

        try {
            const response = await axios.post('https://xtriks.com/api/authorization/function.php', { storeEmail });
            //console.log(response.data);
            response.data.forEach(item => {
                user_id = item.id;
            });
            if(response.data == "error"){
                goto('../login')
            }
        } catch (error) {
            console.error('Error:', error);
        }
    });



    
    
    let schedulerShowing = false;    
    let dateID = "";
    let dateHeading = "";

    let eventName = "";
    let hour = "";
    let minutes = "";
    let hourEND = "";
    let minutesEND = "";
    let user_id;
    let client_id = "1"; //pridavat jine uzivatele nez s id = 1, zatim neudelana funkce

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
            console.log(list); 
        } catch (error) {
            console.error('Error:', error);
        }
	}
    

    
    const closeScheduler = () => {
        schedulerShowing = false;
    }

    const submitAppt = async () => {
		let date_id = dateID;
        try {
            const response = await axios.post('https://xtriks.com/api/insert_data_calendar.php', { date_id ,eventName, hour, minutes, hourEND, minutesEND, user_id, client_id });
            console.log(response.data); 
			readData();
        } catch (error) {
            console.error('Error:', error);
        }
    };

    const handleDeleteEvent = async (id) => {
        try {
            const response = await axios.post('https://xtriks.com/api/delete_data_calendar.php', { id });
            console.log(response.data); 
            readData();
        } catch (error) {
            console.error('Error:', error);
        }
    }
</script>

<main>
    <Sidebar></Sidebar>
    <div class="calendar">
        <Calendar on:click={handleScheduler} />
        {#if schedulerShowing}
            <section transition:fade={{ duration: 125 }}>
                <form method="post" id={dateID} on:submit|preventDefault={submitAppt}>
                    <div id="closer-cont">
                        <span on:click={() => closeScheduler()} class="close" title="Close Modal"><img src={close} alt="close"></span>
                    </div>
                    <header>
                        <h2>Moje události</h2>
                        <input type="text" id="event-input" required placeholder="Název události" bind:value={eventName}>
                        <div id="time-cont">
                            <div class="hrs-mins-cont">
                                <label for="timehour">Čas začátku události</label>
                                <input type="number" class="timeHour-input" name="timeHour" min="1" max="24" step="1" placeholder="H." bind:value={hour}>
                                <span class="time-colon">:</span>
                                <input type="number" class="timeMinutes-input" name="timeMinutes" min="0" max="59" step="1" placeholder="Min." bind:value={minutes}>
                            </div>   
                            <div class="hrs-mins-cont">
                                <label for="timehourEND">Čas konce události</label>
                                <input type="number" class="timeHour-input" name="timeHourEND" min="1" max="24" step="1" placeholder="H." bind:value={hourEND}>
                                <span class="time-colon">:</span>
                                <input type="number" class="timeMinutes-input" name="timeMinutesEND" min="0" max="59" step="1" placeholder="Min." bind:value={minutesEND}>
                            </div>   
                        </div>
                        <div>
                            <button class="addBtn">Přidat</button>
                        </div>  
                    </header>
                	{#each $list as item (item.id)}                        
                        <tr>
                            <td>Název události - {item.eventName}</td>
                            <td>Začátek události - {item.hour}:{item.minutes}</td>
                            <td>Konec události - {item.hour_end}:{item.minutes_end}</td>
                            <td><img src={trash} alt="delete"  role="button" on:click={handleDeleteEvent(item.id)}></td>	
                        </tr>
                    {/each}
                </form>
            </section>
        {/if}
    </div>
</main> 

<style>
    *{
        font-family: sans-serif;
    }
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
    
    h2 {
        margin: 5px 0;
        color: #83464F;
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

    .close img{
        height: 45px;
        width: 45px;
    }

    /* Style the header */
    header {
        display: flex;
        flex-direction: column;
        align-items: center;
        background-color: #e5d5d1;
        padding: 30px 40px;
        color: white;
    }
    
    #time-cont {
        display: flex;
        flex-direction: column;
        justify-content: center;
        text-align: center;
        margin-bottom: 10px;
    }
    
    .hrs-mins-cont {
        display: flex;
        justify-content: space-between;
    }
    
    .time-colon {
        font-size: 3rem;
    }
    
    input[type=number] {
        margin-right: 5px;
        width: 75px;
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
        width: 300px;
        float: left;
        font-size: 1.1rem;
        display: block;
        margin-top: 1rem;
        padding: 0.5rem 0;
        border: 2px solid #83464F;
        border-radius: 10px;
        color: #83464F;
        background-color: #F7E8E9;
        box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
        transition: all 0.3s ease;
        padding-left: 1%;
    }
    .input:focus {
        outline: none;
        border-color: white;
        box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.2);
    }
    input::placeholder{
        color: #83464F;
        outline: none;
    }

    label{
        margin-top: 1.5em;
        margin-right: 1em;
        color: #83464F;
        font-size: 1.1rem;
        font-weight: bold;
    }
    
    .addBtn {
        padding: 10px;
        width: 160%;
        background: #83464F;
        color: #FFF;
        float: left;
        text-align: center;
        font-size: 18px;
        cursor: pointer;
        transition: 0.1s;
        border: 1px solid #83464F;
        border-radius: 10px;
    }

    .addBtn:hover {
        border: 1px solid white;
        background-color:#83464F;
    }   

    .addBtn:active {
        background-color: hsl(168, 76%, 100%);
        color: hsl(168, 76%, 25%);
    }   
    tr {
        text-align: center;
        position: relative;
    }
    td {
        text-align: center;
        padding: 20px;		
        color: #83464F;
    }
    
    td,
    input {
        cursor: pointer;
    }
    td img {
        height: 20px;
        width: 20px;
        cursor: pointer;
    }
    @media screen and (max-width: 700px) {
		.calendar{
            margin-left: 0px;
            margin-right: 0px;
		}
	}
</style>
