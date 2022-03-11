<script>
    import Car from '../lib/car.svelte';
    import Runner from '../lib/runner.svelte';
    import Coin from '../lib/coin.svelte';
    import WelcomeScreen from '../lib/welcomescreen.svelte';
    import GameOver from '../lib/gameover.svelte';
    import AudioPlayer from '../lib/audioplayer.svelte';

    let showwelcome = true, gameover = false, playing = false;
    let character, charWidth = 0, charHeight = 0, charPosY = 0, charPosX = 0;
    let car, car2, car3, car4, carHeight = 0, carWidth = 0, showcar2 = false, showcar3 = false, showcar4 = false; 
    let coin, coinHeight = 0, coinWidth = 0, coinpresent = false, coincount = 0; 
    let bgAudio, coinAudio, gameOverAudio;
    
    $: if(character) {
        charWidth = character.width;
        charHeight =  character.height;
    }
    $: if(car)  {
        carHeight = car.height;
        carWidth = car.width;
    }
    $: hitX = charWidth + charPosX;
    $: if(coin)  {
        coinHeight = coin.height;
        coinWidth = coin.width;
    }
    $: if(bgAudio) {
    }

    function ValidateCrash(event){
        let hitOffsetX = 100;
        let carPosX = event.detail.x + hitOffsetX;   
        let carPosY =  event.detail.y;  
        let endOfHitX = carPosX + carWidth - hitOffsetX;
        let isYHit = (charPosY >= carPosY && charPosY < (carPosY + carHeight));
        if(carPosX <= hitX && endOfHitX > charPosX && isYHit) {
            playing = false;
            gameover = true;
            gameOverAudio.play();
            bgAudio.pause();
        }
    }

    function shouldAddCar(){       
        if(coincount > 30) showcar4 = true; 
        else if(coincount > 20) showcar3 = true;
        else if(coincount > 10) showcar2 = true;
    }
        
    function handleCoinMove(event){
        let coinOffsetY = 50;
        let coinPosX = event.detail.x;
        let coinPosY = event.detail.y + coinOffsetY;
        let endOfHitX = coinPosX + coinWidth;
        let isYHit =  (coinPosY >= charPosY && coinPosY < (charPosY + charHeight));
        if(coinPosX <= hitX && endOfHitX > charPosX && isYHit) {
            coincount+=1;
            coinAudio.play();
            coinpresent = false;
            shouldAddCar();
            setTimeout(()=>{
                coinpresent = true;
            },100);
        } 
    }
    
    function gamestart(e){
        if (e.keyCode == 32) {
            if(playing) return;
            if(!playing) playing = true; 
            if(!coinpresent) coinpresent = true;
            if(gameover) gameover = false;
            if(showwelcome) showwelcome = false;
            coincount = 0;
            bgAudio.play();
            bgAudio.loop=true;
        }
    }
</script>

<svelte:window on:keydown={gamestart} />
<div class="main go-{gameover}">	
    <div class="audioplayers">
        <AudioPlayer src="/static/bg.mp3" bind:player={bgAudio}/>
        <AudioPlayer src="/static/coin.wav" bind:player={coinAudio}/>
        <AudioPlayer src="/static/crash.wav" bind:player={gameOverAudio}/>
    </div>	
    <div class="head">
        Character Position: {charPosX.toFixed()} px, {charPosY.toFixed()} px
        <br>
        <img class="coin" src="/static/coin.gif" alt="score"> <span class="score">{coincount}</span>
    </div>

    {#if playing}
        {#if coinpresent}
        <Coin bind:coin={coin} on:positionchanged={handleCoinMove} />
        {/if}
    <Car bind:car={car} on:positionchanged={ValidateCrash}/>
    {#if showcar2}
    <Car bind:car={car2} on:positionchanged={ValidateCrash}/>
    {/if}
    {#if showcar3}
    <Car bind:car={car3} on:positionchanged={ValidateCrash}/>
    {/if}
    {#if showcar4}
    <Car bind:car={car4} on:positionchanged={ValidateCrash}/>
    {/if}
    {/if}

    {#if !playing && showwelcome}
    <WelcomeScreen/>  
    {/if}

    {#if !gameover} 
    <Runner bind:character={character} bind:x={charPosX} bind:y={charPosY}/>  
    {/if}

    {#if gameover}
    <GameOver score={coincount}/>
    {/if}
</div>

<style>
    @keyframes run {
        from {
            background-position-x: 0;
        }
        to {
            background-position-x: -1500000px;
        }
    }
	.main {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background: black url(/static/bg.jpg) 0 bottom;
        animation: run 3600s linear infinite;
	}
    .go-true{
        animation: none;
    }
    .audioplayers{
        display:none;
    }
    .head{
        margin: 30px
    }
    img.coin{
        position: relative;
        width:60px;
        top: 10px;
    }
    span.score{
        font-size: 3em;
        font-weight:bold;
    }
</style>
