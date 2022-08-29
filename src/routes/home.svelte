<script context="module">
    import { goto } from '$app/navigation';

    const token = !!window.localStorage.getItem('token');

    if(!token){
        goto('/');
    }

</script>

<script lang="ts">

    let iFrame : Partial<HTMLIFrameElement>;


    const handleLogout = () => {
        const data = {
            action : 'logout',
            correo : '',
            password : ''
        }

        iFrame.contentWindow?.postMessage(data,'http://localhost:8080');
        window.localStorage.clear();
        console.log("logout1");
        goto('/');
    }

    window.addEventListener('message', function(message){
        if( message.data.type === 'iframe.loaded' ){
            const data = {
                action : 'isToken',
                correo : '',
                password : ''
            }
            iFrame.contentWindow?.postMessage(data,'http://localhost:8080');
        }else if( message.data.type === 'isToken' ){
            if(!message.data.exist){
                window.localStorage.clear();
                goto('/');
            }
        }else if( message.data.type === 'logout' ){
            console.log("se elimino el token en iframe");
        }
    });

</script>


<button 
    class="px-6 py-2 mt-4 text-white bg-blue-600 rounded-lg hover:bg-blue-900"
    on:click={ handleLogout }
>
    Logout
</button>

<h3>Home Domain 1</h3>


<div class="container">
    <iframe
         bind:this={iFrame}
         class="resp-iframe"
         src="http://localhost:8080" 
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