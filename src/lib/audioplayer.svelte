<script>
    export let player;
    export let src;
</script>

<audio
    bind:this={player}
    {src}
    controls>
    <track kind="captions" />
</audio>