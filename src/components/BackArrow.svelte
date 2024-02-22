<script>
    import icon from "$lib/gallery/angle-circle-left.png"

    export let href; 
</script>
<a href={href}>
    <div class="icon">
        <img src={icon} alt="icon">
    </div>    
</a>
<style>
    .icon{
        height: 50px;
        width: 50px;
        position: absolute;
        left: 0;
        margin-left: 5%;
        margin-top: 1%;
    }
    img{
        height: 45px;
    }
</style>