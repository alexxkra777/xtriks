<script>
    import Account from "../../components/Account.svelte";
</script>

<body>
    <div class="navbar">
        <ul class="menu">
            <li>
                <a href="../../calendar">Kalendář</a>
            </li>
            <li>
                <a href="../../clients">Klienti</a>
            </li>
        </ul>
        <div class="acc">
            <Account></Account>
        </div>
    </div>
            
    <nav role="navigation">
        <div id="menuToggle">
            <input type="checkbox" />
                <span></span>
                <span></span>
                <span></span>
            <ul id="menu">
                <li>
                    <a href="../../calendar">Kalendář</a>
                </li>
                <li>
                    <a href="../../clients">Klienti</a>
                </li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li>
                    <Account></Account>
                </li>
            </ul>
        </div>
    </nav>
</body>

<style>
    body{
        background-color: white;
    }
    a{
        font-family: sans-serif;
    }
    .navbar{
        margin: 0;
        padding: 0;
        width: 300px;
        background-color: #e5d5d1;
        position: fixed;
        height: 100%;
        overflow: auto;
    }
    .menu{
        margin-top: 22em;
        list-style-type: none;
    }
    .menu li{
        margin-top: 2em;
    }
    .menu li a{
        text-decoration: none;
        color: #83464F;
        font-weight: bold;
        opacity:1;
        font-size: 1.5em;
        font-weight: 400;
        transition: 200ms;
    }
    #menuToggle{
        display: none;
    }
    .acc{
        position: fixed;
        bottom: 2em;
        left: 3em;
    }
    @media screen and (max-width: 1400px) {
        .menu{
            margin-top: 15em;
        }  
    }
    
    @media screen and (max-width: 600px) {
        .navbar{
            display: none;
        }
        a {
            text-decoration: none;
            color: white;
            font-weight: bold;
            opacity:1;
            font-size: 1.5em;
            font-weight: 400;
            transition: 200ms;
        }
        a:hover {
            opacity:0.5;
        }
        ul {
            padding: 0;
            list-style-type: none;
        }
        nav {
            background-color: #D3A2A6;
            width: 100%;
            height: 65px;
        }
        #menuToggle {
            display: flex;
            flex-direction: column;
            position: relative;
            top: 25px;
            left: 25px;
            z-index: 1;
            -webkit-user-select: none;
            user-select: none;
        }
        #menuToggle input{
            display: flex;
            width: 40px;
            height: 32px;
            position: absolute;
            cursor: pointer;
            opacity: 0;
            z-index: 2;
        }
        #menuToggle span{
            display: flex;
            width: 29px;
            height: 2px;
            margin-bottom: 5px;
            position: relative;
            background: #ffffff;
            border-radius: 3px;
            z-index: 1;
            transform-origin: 5px 0px;
            transition: transform 0.5s cubic-bezier(0.77,0.2,0.05,1.0),
                    background 0.5s cubic-bezier(0.77,0.2,0.05,1.0),
                    opacity 0.55s ease;
        }

        #menuToggle span:first-child{
            transform-origin: 0% 0%;
        }

        #menuToggle span:nth-last-child(2){
            transform-origin: 0% 100%;
        }

        #menuToggle input:checked ~ span{
            opacity: 1;
            transform: rotate(45deg) translate(-3px, -1px);
            background: #FCE5D8;
        }
        #menuToggle input:checked ~ span:nth-last-child(3){
            opacity: 0;
            transform: rotate(0deg) scale(0.2, 0.2);
        }

        #menuToggle input:checked ~ span:nth-last-child(2){
            transform: rotate(-45deg) translate(0, -1px);
        }

        #menu{
            position: absolute;
            width: 100%;
            box-shadow: 0 0 3px #85888C;
            margin: -50px 0 0 -50px;
            padding: 50px;
            padding-top: 50%;
            padding-bottom: 200%;
            background-color: #D3A2A6;
            transform-origin: 0% 0%;
            transform: translate(-100%, 0);
            transition: transform 0.5s cubic-bezier(0.77,0.2,0.05,1.0);
        }

        #menu li{
            padding: 10px 0;
            transition-delay: 2s;
        }

        #menuToggle input:checked ~ ul{
            transform: none;
        }
    }
</style>