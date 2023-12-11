<script context="module">
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';
</script>
<script>
    onMount(() => {
        goto('/main');
    })
</script>
