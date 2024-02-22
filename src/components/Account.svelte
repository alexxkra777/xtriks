<script>
    import icon from "$lib/gallery/icon.png"
    import { goto } from '$app/navigation';

    function signOut(key) {
        localStorage.removeItem(key);
        goto("../")
    }
</script>
    <button class="icon" on:click={() => signOut('email')}>
        <img src={icon} alt="icon">
    </button>    
<style>
    button {
        border: none;
        background: none;
        padding: 0;
        margin: 0;
        cursor: pointer;
    }
    .icon{
        height: 50px;
        width: 50px;
    }
    img{
        height: 45px;
    }
</style>