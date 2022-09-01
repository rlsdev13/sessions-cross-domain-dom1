<script context="module">
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';

    const token = !!window.localStorage.getItem('token');

    if(token){
        goto('/home');
    }

</script>

<script lang="ts">    
    let iFrame : Partial<HTMLIFrameElement>;

    let correo : string = 'user2@gmail.com';
    let password : string = '123456';

    const handleLogin = () => {
        const data = {
            action : 'login',
            type : 'credentials',
            message : {
                correo,
                password
            }
        }
        
        iFrame.contentWindow?.postMessage(data,'http://192.168.100.23:8080');
    }

    window.addEventListener('message',function(event){

        const { action, type, message } = event.data;
        
        if( action === 'loaded' ){//mensaje cuando el iframe esta cargado
            console.log(action, type, message);
            if(message){//si existe token es recibido aqui
                window.localStorage.setItem('token',message);
                goto('/home');
            }
        }else if( action === 'session'){//mensaje de retorno cuando del servicio de auth
            console.log(action,type,message);
            console.log("token recibido!!!", message)
            window.localStorage.setItem('token',message);
            goto('/home');
        }else if( action === 'error' ){
            console.log(action,type,message);
        }
    });

    onMount(() => {        
    });

</script>

<div class="flex items-center justify-center min-h-screen bg-gray-100">
    <div class="px-8 py-6 mt-4 text-left bg-white shadow-lg">
        <h3 class="text-2xl font-bold text-center">Login Domain 1f</h3>
        <form action="" on:submit|preventDefault={ handleLogin }>
            <div class="mt-4">
                <div>
                    <label class="block" for="email">Email<label>
                    <input 
                        type="text" 
                        placeholder="Email"
                        class="w-full px-4 py-2 mt-2 border rounded-md focus:outline-none focus:ring-1 focus:ring-blue-600"
                        bind:value={ correo }
                    >
                </div>
                <div class="mt-4">
                    <label class="block" for="password">Password<label>
                    <input 
                        type="password" 
                        placeholder="Password"
                        name="password"
                        class="w-full px-4 py-2 mt-2 border rounded-md focus:outline-none focus:ring-1 focus:ring-blue-600" 
                        bind:value={ password }
                    />
                </div>
                <div class="flex items-baseline justify-between">
                    <button class="px-6 py-2 mt-4 text-white bg-blue-600 rounded-lg hover:bg-blue-900">Login</button>
                </div>
            </div>
        </form>
    </div>
</div>

<div class="container">
   <iframe
        bind:this={iFrame}
        class="resp-iframe"
        src="http://192.168.100.23:8080" 
        title="Login"
    >
    </iframe>
</div>

<style>
    .container {
        position: relative;
        overflow: hidden;
        width: 100%;
        height: 100%;
        padding-top: 56.25%; /* 16:9 Aspect Ratio (divide 9 by 16 = 0.5625) */
        overflow-y: hidden;
        overflow-x: hidden;
    }

    /* Then style the iframe to fit in the container div with full height and width */
    .resp-iframe {
        position: absolute;
        top: 0;
        left: 0;
        bottom: 0;
        right: 0;
        width: 100%;
        height: 100%;
        border: none;
        overflow-y: hidden;
        overflow-x: hidden;
    }
</style>