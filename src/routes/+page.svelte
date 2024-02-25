<script>
    import NavBar from "../components/NavBar.svelte";
    import screenCalendar from "$lib/gallery/screen_calendar.png";
</script>


<NavBar></NavBar>

<div class="header">
    <h1>CRM systém</h1>
    <h2>Pro váš přehled, vše krásně na jednom místě</h2>
    <div class="div_screen_calendar">
        <img src={screenCalendar} alt="screen_calendar" class="screen_calendar">
    </div>
</div>

<style>
    *{
        font-family: sans-serif;
    }
    .header{
        width: 100%;
        display: flex;
        flex-direction: column;
    }
    .header h1{}
    .div_screen_calendar{
        margin-right: 5em;
    }
    .screen_calendar{
        width: 50em;
        float: right;
        border: 20px solid #83464F;
        border-radius: 20px;
    }
</style>