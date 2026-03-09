<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Happy Birthday + Фотоальбом</title>
<style>
html,body{
  margin:0;
  padding:0;
  background:black;
  font-family:Arial;
  color:white;
  min-height:100vh;
  display:flex;
  justify-content:center;
  align-items:center;
  overflow:hidden;
}

/* MATRIX */
canvas{
  position:fixed;
  top:0;
  left:0;
  width:100%;
  height:100%;
  z-index:0;
}

/* CONTENT */
#content{
  position:relative;
  z-index:2;
  display:flex;
  flex-direction:column;
  align-items:center;
  text-align:center;
}

.big{
  font-size:80px;
  color:#ff4da6;
  text-shadow:0 0 25px #ff4da6;
}

.word{
  font-size:50px;
  display:none;
  color:#ff66cc;
  text-shadow:0 0 20px #ff66cc;
  animation:fadeInOut 2s ease forwards;
}

@keyframes fadeInOut{
  0%,100%{opacity:0;}
  10%,90%{opacity:1;}
}

#newGif{
  display:none;
  margin-top:20px;
  width:200px;
  opacity:0;
  transition:opacity 0.8s;
}

/* ALBUM */
.book {
  display:none;
  position: relative;
  width:400px;
  height:210px;
  perspective:1500px;
  margin-top:30px;
  border-radius:12px;
  overflow: hidden;
}

.staticLeft, .flipPage {
  position: absolute;
  width:45%;
  height:90%;
  top:5%;
  overflow: hidden;
  border-radius:12px;
  background:white;
}

.staticLeft {
  left:5%;
  z-index:1;
}

.flipPage {
  right:5%;
  transform-origin: left;
  cursor: pointer;
  transform-style: preserve-3d;
  z-index:2;
}

.front, .back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius:12px;
}

.back {
  transform: rotateY(180deg);
}

.staticLeft img, .flipPage img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius:12px;
}

.flipPage.flipping {
  animation: flip 1s forwards;
}

@keyframes flip {
  0% { transform: rotateY(0deg); }
  100% { transform: rotateY(-180deg); }
}

.cover {
  position: absolute;
  width: 100%;
  height: 100%;
  background: #ff4da6;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 30px;
  cursor: pointer;
  z-index: 10;
  border-radius:12px;
  box-shadow:0 0 20px #ff4da6;
}
</style>
</head>
<body>

<canvas id="matrix"></canvas>

<div id="content">

  <div id="count" class="big" style="display:none;">3</div>

  <div id="w1" class="word">Happy</div>
  <div id="w2" class="word">Birthday</div>
  <div id="w3" class="word">to</div>
  <div id="w4" class="word">you</div>
  <div id="heart" class="word">❤️</div>

  <img id="newGif"
       src="https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExaGtyYmsxMTM0YjB4NDQ5Y3dwd3NsN3JtcjQ2eHN0aHJ4bGthcDN3MyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9cw/COxdWdYHHtPgWvDb5k/giphy.gif">

  <!-- АЛЬБОМ -->
  <div class="book" id="album">
    <div class="cover" id="cover">📖 Открыть альбом</div>
    <div class="staticLeft">
      <img id="leftImg">
    </div>
    <div class="flipPage" id="page">
      <div class="front">
        <img id="frontImg">
      </div>
      <div class="back">
        <img id="backImg">
      </div>
    </div>
  </div>

</div>

<script>
// MATRIX
const canvas=document.getElementById("matrix");
const ctx=canvas.getContext("2d");
const letters="0123456789";
const fontSize=16;
let columns;
let drops=[];
function resize(){
  canvas.width=window.innerWidth;
  canvas.height=window.innerHeight;
  columns=Math.floor(canvas.width/fontSize);
  drops=[]; for(let i=0;i<columns;i++) drops[i]=1;
}
resize();
window.addEventListener("resize",resize);
function draw(){
  ctx.fillStyle="rgba(0,0,0,0.05)";
  ctx.fillRect(0,0,canvas.width,canvas.height);
  ctx.fillStyle="#ff4da6";
  ctx.font=fontSize+"px monospace";
  for(let i=0;i<drops.length;i++){
    let text=letters[Math.floor(Math.random()*letters.length)];
    ctx.fillText(text,i*fontSize,drops[i]*fontSize);
    if(drops[i]*fontSize>canvas.height && Math.random()>0.975) drops[i]=0;
    drops[i]++;
  }
}
setInterval(draw,33);

// WORDS COUNTDOWN
let count=3;
const c=document.getElementById("count");
const w1=document.getElementById("w1");
const w2=document.getElementById("w2");
const w3=document.getElementById("w3");
const w4=document.getElementById("w4");
const heart=document.getElementById("heart");
const gif=document.getElementById("newGif");
const album=document.getElementById("album");
const cover=document.getElementById("cover");
const leftImg=document.getElementById("leftImg");
const frontImg=document.getElementById("frontImg");
const backImg=document.getElementById("backImg");
const page=document.getElementById("page");

const photos = [
  "https://picsum.photos/500/260?1",
  "https://picsum.photos/500/260?2",
  "https://picsum.photos/500/260?3",
  "https://picsum.photos/500/260?4",
  "https://picsum.photos/500/260?5",
  "https://picsum.photos/500/260?6",
  "https://picsum.photos/500/260?7",
  "https://picsum.photos/500/260?8"
];

let index = 0;

function updatePhotos(){
  leftImg.src = photos[index];
  frontImg.src = photos[(index + 1) % photos.length];
  backImg.src = photos[(index + 2) % photos.length];
}

setTimeout(()=>{
  c.style.display="block";
  const timer=setInterval(()=>{
    count--;
    if(count>0) c.innerText=count;
    else{
      clearInterval(timer);
      c.style.display="none";
      showWords();
    }
  },1000);
},3000);

function showWords(){
  setTimeout(()=>{ w1.style.display="block"; },500);
  setTimeout(()=>{ w1.style.display="none"; w2.style.display="block"; },2000);
  setTimeout(()=>{ w2.style.display="none"; w3.style.display="block"; },3500);
  setTimeout(()=>{ w3.style.display="none"; w4.style.display="block"; },5000);
  setTimeout(()=>{ heart.style.display="block"; },6500);

  setTimeout(()=>{
    w4.style.display="none";
    heart.style.display="none";
    gif.style.display="block";
    gif.style.opacity="1";
  },8500);

  setTimeout(()=>{
    gif.style.opacity="0";
    setTimeout(()=>{ gif.style.display="none"; },500);
    album.style.display="block";
  },10500);
}

// ALBUM FLIP
cover.onclick = () => {
  cover.style.display = "none";
  updatePhotos();
}

page.onclick = () => {
  backImg.src = photos[(index + 2) % photos.length];
  page.classList.add("flipping");

  setTimeout(() => {
    index = (index + 2) % photos.length;
    leftImg.src = photos[index];
    frontImg.src = photos[(index + 1) % photos.length];
    page.classList.remove("flipping");
  }, 1000);
}
</script>

</body>
</html>
