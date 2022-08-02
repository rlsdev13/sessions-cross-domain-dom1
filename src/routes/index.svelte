<script lang="ts">
    import { onMount } from 'svelte';
    
    let iFrame : Partial<HTMLIFrameElement>;
    
    const data = "holla session 1";
    
    onMount( async() => {
        iFrame = document.getElementById('iframe-id')!;
        iFrame.contentWindow!.postMessage(data);
        window.localStorage.setItem('sitio1',data);
        
    });



</script>

<div class="container">
   <iframe
        id="iframe-id"
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