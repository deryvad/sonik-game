<script>
    export let score;
    let key = "runner_score";
    let personalbest = localStorage.getItem(key) || 0;
    $: {
        if(score > personalbest) {
            localStorage.setItem(key, score);
            personalbest = score;
        }
    }
</script>
<div class="modal">
    <div class="cont">
        <span class="title">
            GAME OVER
        </span>
        <br>    
        <br>    
        <span>Your score is: <b>{score}</b></span>
        <br>    
        <span>Your personal best: <b>{personalbest}</b></span>
        <br>    
        <br>    
        <span class="subtitle">Press the <span style="color: red">SPACE BAR</span> try again</span>  
    </div>
</div>

<style>
    .cont{
        position: relative;
        top: 20%;
    }
    .cont > .title{
        color:rgb(155, 46, 46);
        font-size: 5em;
        font-weight: bold;
        margin-left: 60px;
    }
    .cont > span{
        color:rgb(155, 46, 46);
        font-size: 2em;
        margin-left: 60px;
    }
    .modal{
        position: fixed;
        top: 10%;
        bottom: 10%;
        left: 10%;
        right: 10%;
		background: black url(/static/gameover.jpg) right bottom;
        background-size: cover;
        box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
        border: 5px solid black;
        border-radius: 25px;
    }
</style>