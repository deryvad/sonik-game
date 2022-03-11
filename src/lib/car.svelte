<script>
    import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

    export let car;
    let interval;
    try{clearInterval(interval);}
    catch(e){}
    
    //dispatch car position every 100ms
    interval = setInterval(()=>{
        if(car){
            let cPos = car.getBoundingClientRect();
            dispatch('positionchanged', {
                x: cPos.left,
                y: cPos.top
            });
        }
    }, 100); 
</script>

<img src="/static/car.gif" alt="car" bind:this={car}>

<style>
    @keyframes drive {
        from {
            right: -2000px;
        }
        to {
            right: 4000px;
        }
    }

    img{
        width: 300px;
        height: 300px;
        position: fixed;
        bottom: 20px;
        right:-2000px;
        animation: drive 6s linear infinite;
    }
</style>