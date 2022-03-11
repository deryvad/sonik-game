<script>
    import { fade } from 'svelte/transition';
	import { elasticOut } from 'svelte/easing';
    let animationduration = 2000;

	let titlevisible = false;
    setTimeout(()=>{titlevisible = true;},100)
    function spin(node, { duration }) {
		return {
			duration,
			css: t => {
				const eased = elasticOut(t);
				return `transform: scale(${eased}) rotate(${eased *720}deg);`
			}
		};
	}
	let subtitlevisible = false;
    setTimeout(()=>{subtitlevisible = true;},animationduration);

</script>

<div class="modal">
    {#if titlevisible}
    <div class="centered" in:spin="{{duration: animationduration}}" out:fade>
		<span class="title">SONIK</span>
        <br>
	</div>
    {/if}
    {#if subtitlevisible}
    <span class="subtitle centered" in:fade out:fade>Press the <span style="color: red">SPACE BAR</span> to start</span>    
    {/if}
</div>

<style>
    .centered {
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate(-50%,-50%);
	}

	span.title {
		position: absolute;
		transform: translate(-50%,-50%);
		font-size: 5em;
        font-weight: bold;
        color:rgb(47, 58, 211);
	}
    
    span.subtitle{
		position: absolute;
		transform: translate(-50%,70%);
		font-size: 2em;
    }

    .modal{
        position: fixed;
        top: 70px;
        bottom: 45%;
        left: 20%;
        right: 20%;
        background: rgba(255,255,255,0.9);
        box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
        border: 5px solid black;
        border-radius: 25px;
        overflow: hidden;
    }
</style>