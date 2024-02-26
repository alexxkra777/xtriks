<script>
    import NavBar from "../components/NavBar.svelte";
    import screenCalendar from "$lib/gallery/screen_calendar.png";
</script>


<NavBar></NavBar>

<div class="header">
    <h1>CRM systém<br><span class="h2">Pro váš přehled, vše krásně na jednom místě</span></h1>
    <div class="div_screen_calendar">
        <img src={screenCalendar} alt="screen_calendar" class="screen_calendar">
    </div>
</div>
<div class="about">
    <h2>O projektů</h2>
    <h3>Tetno projekt by měl výhradně sloužit pro lidi z oborů krásy a zdravý o tělo.</h3>
    <p>Xtriks CRM systém má v sobě zaimplepované služby pro ukládání uživatelských infromací a vytváření události ve svém kalendáři.<br><br> Každý uživatel má svůj kalendář, kde si může zadávat čas začátku události a čas konce události. Také každý uživatel má svůj list z uživatelů, kde si může znamenat o nich prioritní infromace.<br><br> Následně klienti, po tom co si jim bude vytvořený profil, se mohou přihlásit jako kleinti a uvidí zakládní infromace o vás, aby vás mohli kontaktovat.</p>
</div>
<div class="devoleper">
    <h2>O tvůrci projektů</h2>
    <p>V dnešní době mnoho malých i velkých podniků používá pro svůj přehled CRM systém (řízení vztahů se zákazníky). V době koronaviru se začala moje starší sestra zajímat o kosmetologii a chtěla mít přehled o všech svých klientech. Když jsme spolu začali hledat nějaký takový systém, který by byl jednoduchý k používání, přehledný a cenově dostupný, tak jsme nenašli nic, co by se nám líbilo.</p>
    <p>Začal jsem přemýšlet, jak to udělat technicky,  jaké funkce by aplikace měla mít a vymýšlel celou logiku aplikace. Provedl jsem několik konzultací s kosmetoložky, abych čerpal z jejich zkušeností, co by potřebovali k užívání. Vytvořil jsem si v hlavě obrázek o možném výsledků, má představa je: stránka s  informacemi o klientovi (narozeniny, jméno, nemoci, těhotenství, kontakty, …) a rezervační systém.</p>
    <p>Mojí největší motivací na projektu je, že bych jej chtěl dále zlepšovat a prodávat licenci na daný systém. V ideálních podmínkách by software mohli začít používat všichni dříve zmíněné potenciální uživatelé a také doktoři. Klienti by měli přehledně v jedné aplikaci všechny svoje kosmetoložky, doktory atd, a nemuseli by za to nic platit, protože licence by platili jen uživatelé CRM. </p>
</div>
<div class="contact">
    <h2>Kontakty</h2>
    <ul>
        <li>Email - krayual20@sps-prosek.cz</li>
    </ul>
</div>
<div class="footer">
    <p>Alexandr Krajuškin 2024
</div>

<style>
    *{
        font-family: sans-serif;
    }
    .header{
        width: 100%;
        display: flex;
        flex-direction: row;
        margin-top: 2em;
    }
    .header h1{
        color: #83464F;
        margin-top: 3em;
        margin-right: 0.5em;
        margin-left: 0.2em;
    }
    .header .h2{
        font-size: 23px;
    }
    .div_screen_calendar{
        margin-right: 0.5em;
    }
    .screen_calendar{
        width: 30em;
        float: right;
        border: 15px solid #83464F;
        border-radius: 20px;
    }
    .about{
        margin-top: 5em;
        width: 100%;
        height: 23em;
        background-color: #83464F;
        display: flex;
        flex-direction: column;
        align-items: flex-end
    }
    .about h2{
        font-size: 35px;
        float: right;
        color: white;
        margin-right: 4em;
    }
    .about h3{
        color: white;
        float: right;
        font-size: 16px;
        margin-right: 4em;
    }
    .about p{
        color: white;
        float: right;
        width: 60%;
        margin-right: 4em;
    }
    .devoleper{
        margin-top: 5em;
        width: 100%;
        height: 30em;
        display: flex;
        flex-direction: column;
        align-items: flex-start
    }
    .devoleper h2{
        font-size: 35px;
        float: left;
        color: #83464F;
        margin-left: 4em;
    }
    .devoleper h3{
        color: #83464F;
        float: left;
        font-size: 16px;
        margin-left: 4em;
    }
    .devoleper p{
        color: #83464F;
        float: left;
        width: 60%;
        margin-left: 4em;
    }
    .contact{
        margin-top: 5em;
        width: 100%;
        height: 15em;
        display: flex;
        flex-direction: column;
        align-items: center;
        background-color: #83464F
    }
    .contact h2{
        font-size: 35px;
        float: left;
        color: white;
    }
    ul{
        list-style-type: none;
    }
    ul li{
        color: white;
    }
    .footer{
        width: 100%;
        height: 3em;
        background-color: #D3A2A6;
        margin-top:-1em;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .footer p{
        color: white;
    }
    @media screen and (max-width: 600px) {
        .header{
            flex-direction: column;
        }
        .screen_calendar{
        width: 20em;
        border: 7.5px solid #83464F;
        border-radius: 20px;
    }
    }

</style>