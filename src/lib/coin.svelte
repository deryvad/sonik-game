<script>
    import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

    export let coin;
    let yPos = randomIntFromInterval(25, 55);
    $: if(coin) {
        coin.style.bottom = yPos + "%";
    }
    function randomIntFromInterval(min, max) { 
        return Math.floor(Math.random() * (max - min + 1) + min)
    }
    let interval;
    try{clearInterval(interval);}
    catch(e){}    
    //dispatch coin position every 100ms
    interval = setInterval(()=>{
        if(coin){
            let cPos = coin.getBoundingClientRect();
            if(cPos.left < 0) yPos = randomIntFromInterval(25, 30); 
            dispatch('positionchanged', {
                x: cPos.left,
                y: cPos.top,
                coin: coin
            });
        }
    }, 100); 
</script>

<img src="/static/coin.gif"  alt="coin" bind:this={coin}>

<style>
    @keyframes move {
        from {
            right: -2000px;
        }
        to {
            right: 4000px;
        }
    }

    img{
        width: 60px;
        height: 60px;
        position:fixed; 
        right:-2000px;
        animation: move 10s linear infinite;
    }
</style>