<script>
    import { onMount } from 'svelte';
    export let player;
    export let src;
    let audio;
    onMount(() => {
      player.volume= 0.05;
    }) 
</script>

<audio
    bind:this={player}
    {src}
    controls>
    <track kind="captions" />
</audio>