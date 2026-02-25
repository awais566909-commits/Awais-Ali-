# Awais-Ali-<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Romantic Love Page</title>

<style>
body{
    margin:0;
    overflow:hidden;
    font-family:Arial, sans-serif;
    background:linear-gradient(135deg,#ffb6c1,#ffc0cb,#ffe6f0);
    display:flex;
    justify-content:center;
    align-items:center;
    height:100vh;
}

/* Start Screen */
.start-screen{
    position:absolute;
    text-align:center;
    z-index:1000;
}
.heart-btn{
    font-size:60px;
    background:none;
    border:none;
    cursor:pointer;
    animation:pulse 1.5s infinite;
}
@keyframes pulse{
    0%{transform:scale(1);}
    50%{transform:scale(1.2);}
    100%{transform:scale(1);}
}
.start-text{
    margin-top:10px;
    font-size:18px;
    color:#ff0066;
}

/* Falling Hearts */
.heart{
    position:fixed;
    top:-50px;
    pointer-events:none;
}

/* Character */
.character-container{
    position:absolute;
    text-align:center;
    opacity:0;
    transition:opacity 1s;
}
.character{
    font-size:90px;
}
.speech{
    background:white;
    padding:15px 20px;
    border-radius:25px;
    margin-top:15px;
    display:inline-block;
    max-width:320px;
    color:#ff0066;
    font-weight:bold;
}

/* Final Message */
.final-message{
    position:absolute;
    font-size:22px;
    color:#ff0066;
    font-weight:bold;
    opacity:0;
    transition:opacity 2s;
}

/* I LOVE YOU texts */
.love-text{
    position:fixed;
    font-weight:bold;
    white-space:nowrap;
    pointer-events:none;
}

/* Big Heart */
.big-heart{
    position:absolute;
    width:60vw;
    height:60vw;
    max-width:400px;
    max-height:400px;
    background:red;
    border-radius:50%;
    display:flex;
    justify-content:center;
    align-items:center;
    color:white;
    font-weight:bold;
    font-size:28px;
    text-align:center;
    opacity:0;
    transition:opacity 2s;
    box-shadow:0 0 60px #ff0000;
    animation:pulseBig 1.5s infinite;
}
@keyframes pulseBig{
    0%{transform:scale(1);}
    50%{transform:scale(1.1);}
    100%{transform:scale(1);}
}

/* Responsive */
@media(max-width:500px){
.character{font-size:70px;}
.speech{font-size:14px;}
.big-heart{width:75vw;height:75vw;font-size:20px;}
}
</style>
</head>

<body>

<div class="start-screen" id="startScreen">
    <button class="heart-btn" onclick="startExperience()">❤️</button>
    <div class="start-text">Tap to Start 💖</div>
</div>

<audio id="bgMusic" loop>
    <source src="https://cdn.pixabay.com/download/audio/2022/07/20/audio_524ec1349c.mp3?filename=romantic-lofi-piano-112377.mp3" type="audio/mpeg">
</audio>

<div id="container" class="character-container">
    <div id="character" class="character"></div>
    <div id="speech" class="speech"></div>
</div>

<div id="finalMessage" class="final-message">
    I am sorry to my lovely sweet little baby marie 💕
</div>

<div id="bigHeart" class="big-heart">
    My queen I rab you 💖
</div>

<script>

const characters=[
{emoji:"🧑‍🎤",text:"I am so sorry for my behavior my queen 👑"},
{emoji:"🥺",text:"I am sorry cutie potato I will not do it again"},
{emoji:"👶",text:"I apologize I am a toddler so don't be mad at me"},
{emoji:"🤴",text:"I am sorry princess 💋 now forget what happens"},
{emoji:"😊",text:"Let's just start a new day with a smile and again that lovely talk ❤️"}
];

let index=0;
const container=document.getElementById("container");
const character=document.getElementById("character");
const speech=document.getElementById("speech");
const finalMessage=document.getElementById("finalMessage");
const bigHeart=document.getElementById("bigHeart");

function startExperience(){
    document.getElementById("startScreen").style.display="none";
    const music=document.getElementById("bgMusic");
    music.volume=0.2;
    music.play();
    showCharacter();
    setInterval(createHeart,200);
}

/* Characters */
function showCharacter(){
    container.style.opacity=0;
    setTimeout(()=>{
        character.textContent=characters[index].emoji;
        speech.textContent=characters[index].text;
        container.style.opacity=1;

        setTimeout(()=>{
            container.style.opacity=0;
            index++;
            if(index<characters.length){
                setTimeout(showCharacter,1000);
            }else{
                setTimeout(showFinalMessage,1500);
            }
        },3000);
    },800);
}

function showFinalMessage(){
    finalMessage.style.opacity=1;
    setTimeout(showLoveTexts,3000);
}

/* Falling I LOVE YOU from TOP */
function showLoveTexts(){
    finalMessage.style.opacity=0;

    const duration=65000;
    const startTime=Date.now();

    function createFallingLoveText(){
        const div=document.createElement("div");
        div.classList.add("love-text");
        div.innerText="I LOVE YOU ❤️";

        div.style.top="-100px";
        div.style.left=Math.random()*100+"vw";

        let baseSize=18+Math.random()*20;
        div.style.fontSize=baseSize+"px";

        const colors=["#ff0000","#ff3399","#ff4d88","#ff66b2"];
        div.style.color=colors[Math.floor(Math.random()*colors.length)];

        document.body.appendChild(div);

        const fallDuration=4000+Math.random()*2000;
        const start=Date.now();
        const swayAmount=Math.random()*40-20;
        const rotation=Math.random()*360;

        function animate(){
            const elapsed=Date.now()-start;
            const progress=elapsed/fallDuration;

            if(progress<1){
                const y=progress*window.innerHeight;
                const sway=Math.sin(progress*Math.PI*2)*swayAmount;

                div.style.top=y+"px";
                div.style.transform=`translateX(${sway}px) rotate(${rotation*progress}deg) scale(${1+progress*0.3})`;
                requestAnimationFrame(animate);
            }else{
                div.remove();
            }
        }
        animate();
    }

    function spawnLoop(){
        const elapsed=Date.now()-startTime;
        if(elapsed<duration){
            for(let i=0;i<3;i++){
                createFallingLoveText();
            }
            setTimeout(spawnLoop,200);
        }else{
            showBigHeart();
        }
    }

    spawnLoop();
}

/* Falling Hearts */
function createHeart(){
    const heart=document.createElement("div");
    heart.classList.add("heart");
    heart.innerHTML="❤️";
    heart.style.left=Math.random()*100+"vw";
    heart.style.fontSize=(10+Math.random()*20)+"px";
    document.body.appendChild(heart);

    const duration=4000+Math.random()*2000;
    const start=Date.now();

    function animate(){
        const elapsed=Date.now()-start;
        const progress=elapsed/duration;
        if(progress<1){
            heart.style.top=progress*window.innerHeight+"px";
            heart.style.transform=`rotate(${progress*360}deg)`;
            requestAnimationFrame(animate);
        }else{
            heart.remove();
        }
    }
    animate();
}

function showBigHeart(){
    bigHeart.style.opacity=1;
}

</script>
</body>
</html>
