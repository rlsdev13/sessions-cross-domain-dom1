<script context="module">
    import { goto } from '$app/navigation';

    const token = !!window.localStorage.getItem('token');

    if(!token){
        goto('/');
    }

</script>

<h3>Home</h3>