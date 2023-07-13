<script>
	import { authHandlers } from "../stores/store";

    let email='';
    let password='';
    let confirmPass='';
    let error =false;
    let register=false;
     let authen=false;
    async function handleAuthen(){
       if(authen){return;}
        
        if(!email || !password || (register && !confirmPass)){
            error=true;
            return;
        }
        authen=true;
        try {
            if(!register){
            await authHandlers.login(email,password);
        }
        else{
            await authHandlers.signup(email,password);
        }
            
        } catch (err) {
            console.log("There was an auth error", err);
            error = true;
            authenticating = false;
        }
    
    }
    
    function handleRegister(){
        register=!register;
    }
</script>

<div class="auth"> 
 <form>
    <h1>{register?"Register":"Login"}</h1>
    {#if error}
    <p class="error">Info not correct</p>
    {/if}
   
    <label>
        <p class={email? "above":"center"}>Email</p>
        <input type="email"
         bind:value={email} placeholder="email">
    </label>
    <label>
        <p class={password? "above":"center"}>Password</p>
        <input type="password" 
        bind:value={password} placeholder="password">
    </label>
    {#if register}
    <label>
        <p class={confirmPass? "above":"center"}>Confirm Password</p>
        <input type="password" 
        bind:value={confirmPass}
        placeholder=" confirm password">
    </label>
    {/if}
    
    <button on:click={handleAuthen} type="button">SUBMIT</button>
 </form>
 
 <div class="options">
    <p>Or</p>
    {#if register}
        <div><p>Already have an account?</p>
            <p on:click={handleRegister} on:keydown={()=>{}}>Login</p>
        </div>
        {:else}
        <div><p>Don't have an account?</p>
            <p  on:click={handleRegister} on:keydown={()=>{}}>Register</p>
            </div>
    {/if}
 </div>

</div>



<style>
    .auth{
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        flex: .5;
        padding: 25px;
    }
    form{
        display: flex;
        flex-direction: column;
        gap:14px;
    }
    form,.options{
        width: 400px;
        max-width: 100%;
        margin:0 auto;
    }
    h1{
        text-align: center;
        font-size: 3rem;
    }
    form input{
        border: none;
        background: transparent;
        color: white;
        padding: 8px 15px
    }
    form input:focus{
        border:none;
        outline: none;
    }
    form label{
        position: relative;
        border: 1px solid black;
        
        border-radius: 4px;
    }
    form button{
        background:rgb(28, 0, 41);
        color: white;
        border: none;
        padding: 10px 0px;
        border-radius: 5px;
        cursor: pointer;
    }
    form  button:hover{
        background: red;
    }

    .above,.center{
        position: absolute;
        transform: translateY(-50%);
        pointer-events: none;
        color: white;
        border-radius: 4px;
        padding: 0 6px;
        font-size: 0.8rem;

    }
    .center{
        top: 50%;
        left: 6px;
        border: 1px solid transparent;
        opacity: 0;
    }
    .above{
        top: 0;
        left: 25px;
        background: purple;
        border: .8px solid black;
        font-size: .6rem;
    }

    .options {
        padding: 14px 0;
        overflow: hidden;
        font-size: 0.9rem;
        display: flex;
        flex-direction: column;
        gap: 4px;
    }
    .options>p{
        position: relative;
        text-align: center;
        width: fit-content;
        margin: 0 auto;
        padding: 0 8px;
    }
    .options > p::after,
    .options > p::before {
        position: absolute;
        cursor: pointer;
        content: "";
        top: 50%;
        transform: translateY(-50%);
        width: 100vw;
        height: 1.5px;
        
    }

    .options > p::after {
        right: 100%;
       cursor:pointer;
    }

    .options > p::before {
        left: 100%;
        cursor: pointer;
    }
    .options div {
        display: flex;
        align-items: center;
        gap: 8px;
        justify-content: center;
    }

    .options div p:last-of-type {
        color: cyan;
        cursor: pointer;
    }

    .error{
        text-align: center;
        color: red;
    }
</style>