<script>    
    //export these so parent can bind the object   
    export let character;   
    export let x;
    export let y;

    let gojump = false;
    let animating = false;
    let airtime = 500;
    let posInterval;
    function trackPosition(){
        try{clearInterval(posInterval);}
        catch(e){}
        //change positionY every 100ms
        posInterval = setInterval(()=>{
            let cPos = character.getBoundingClientRect();
            if(y == cPos.top) clearInterval(posInterval);
            else {
                y = cPos.top; 
                x = cPos.left;
            } 
        }, 100);
    }

	function handleKeydown(e) {
		//when space key is pressed
        if(animating) return;
		if (e.keyCode == 32) {
			gojump = true;
            animating = true;
            trackPosition();
			setTimeout(() => {
				gojump = false;
			}, airtime);
            setTimeout(() => {
				animating = false;
			}, (airtime * 2) - 100);
            
		}
	};
    
</script>

<svelte:window on:keydown={handleKeydown} />
<img class="{gojump ? 'jumping':''}" src="runner.gif" alt="runner" bind:this={character}/>

<style>    
	img {
		width: 150px;
		height: 150px;
		position: fixed;
		left: 50%;
		bottom: 120px;
		transform: translateX(-50%);
        transition: all 0.5s ease-in-out;
	}
    img.jumping{
		bottom: 50%;
    }
</style>