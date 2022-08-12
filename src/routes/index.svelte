<script context="module">
    
</script>

<script lang="ts">
    import { onMount } from 'svelte';
    import { goto } from '$app/navigation';
    
    let iFrame : Partial<HTMLIFrameElement>;

    const data = {
        action : 'save',
        key : 'token',
        value : "2a1sd5sa1da1s4c21x321c"
    }

    window.addEventListener('message',function(message){
        if(message.data.type=="session.loaded"){
            window.localStorage.setItem('token',message.data.token)
            goto('/home');
        }
    });

    onMount(() => {
        document.addEventListener('readystatechange', () => {
            setTimeout(() => {
                iFrame.contentWindow?.postMessage(data,'http://localhost:8080');
            },100);
        });
    });


    const handleLoad = () => {
        iFrame.contentWindow?.postMessage(data,'http://localhost:8080');
    }

</script>

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