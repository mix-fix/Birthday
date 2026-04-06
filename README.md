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
  width:600px;
  height:350px;
  perspective:2000px;
  margin-top:30px;
}

#bookPages {
  width:100%;
  height:100%;
  position:relative;
}

.page {
  width:50%;
  height:100%;
  position:absolute;
  right:0;
  top:0;
  transform-origin:left;
  transform-style:preserve-3d;
  transition:transform 1s;
  cursor:pointer;
}

.page-front, .page-back {
  position:absolute;
  width:100%;
  height:100%;
  backface-visibility:hidden;
}

.page-back {
  transform:rotateY(180deg);
}

.page img {
  width:100%;
  height:100%;
  object-fit:cover;
}

.page.flipped {
  transform:rotateY(-180deg);
}

.cover {
  position:absolute;
  width:100%;
  height:100%;
  background:#ff4da6;
  display:flex;
  justify-content:center;
  align-items:center;
  font-size:30px;
  z-index:10;
  cursor:pointer;
}

.leftSide {
  position:absolute;
  left:0;
  top:0;
  width:50%;
  height:100%;
  background:white;
  border-radius:10px 0 0 10px;
  box-shadow: inset 0 0 20px rgba(0,0,0,0.2);
  z-index:0;
} 

#photoHeart{
  display:none;
  position:absolute;
  width:400px;
  height:400px;
}

#photoHeart img{
  position:absolute;
  width:60px;
  height:60px;
  object-fit:cover;
  border-radius:10px;
  transition:all 1s ease;
}

/* Анимация текста после альбома */
@keyframes birthdayText {
  0% { opacity: 0; transform: translate(-50%, -50%) scale(0.5) rotate(-10deg); }
  50% { opacity: 1; transform: translate(-50%, -50%) scale(1.2) rotate(5deg); }
  100% { opacity: 1; transform: translate(-50%, -50%) scale(1) rotate(0deg); }
}

#finalText.show {
  display: block;
  animation: birthdayText 2s ease forwards;
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

<audio id="music" src="https://cdn.pixabay.com/download/audio/2023/03/26/audio_a2c0b29504.mp3?filename=modern-classical-happy-birthday-to-you-370804.mp3" loop></audio>

  <!-- АЛЬБОМ -->
<div class="book" id="album">
  <div class="cover" id="cover">📖 Открыть альбом</div>
  <div id="bookPages">
  <div class="leftSide"></div>
</div>
</div>
<div id="photoHeart"></div>

<div id="finalText" style="
  display:none;
  position:absolute;
  font-size:60px;
  color:#ff4da6;
  text-shadow:0 0 20px #ff4da6;
  top:50%;
  left:50%;
  transform:translate(-50%, -50%);
">
  С ДНЁМ РОЖДЕНИЯ 🎉
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
const music = document.getElementById("music");

const photos = [
  "https://image2url.com/r2/default/images/1775056899876-6c0309a2-0378-4e11-8f5d-b1874a11d8d3.png",
  "https://image2url.com/r2/default/images/1775056964890-d19bb721-fb48-4d07-9567-a4828f30f3ae.jpg",
  "https://image2url.com/r2/default/images/1775056991689-a6b7e775-9197-44d3-bd37-337900f40bc2.jpg",
  "https://image2url.com/r2/default/images/1775057057416-ad727d57-e424-41de-b64c-5c17db06bd9e.png",
  "https://image2url.com/r2/default/images/1775057092991-52765f3c-b9b8-4b7d-8f74-0aa3178f2bde.png",
  "https://image2url.com/r2/default/images/1775501474046-7c3db85b-fa05-4b15-84b1-268a1a993a9b.jpg",
];

const bookPages = document.getElementById("bookPages");

function createBook(){
  bookPages.innerHTML = "";

  for(let i = 0; i < photos.length; i += 2){

    const page = document.createElement("div");
    page.className = "page";
    page.style.zIndex = photos.length - i;

    const front = photos[i];
    const back = photos[i+1] || photos[i];

    page.innerHTML = `
      <div class="page-front">
        <img src="${front}">
      </div>
      <div class="page-back">
        <img src="${back}">
      </div>
    `;

 page.onclick = () => {
  if(page.classList.contains("flipped")){
    page.classList.remove("flipped");
    page.style.zIndex = photos.length - i;
  } else {
    page.classList.add("flipped");
    page.style.zIndex = i;
  }

  checkAllFlipped();
};

    bookPages.appendChild(page);
  }
}



setTimeout(()=>{
  c.style.display="block";

  const timer = setInterval(()=>{
    count--;

    if(count > 0){
      c.innerText = count;
    } else {
      clearInterval(timer);
      c.style.display = "none";

      music.volume = 0;
music.play();

let vol = 0;
const fade = setInterval(() => {
  if(vol < 1){
  vol += 0.05;
  if(vol > 1) vol = 1; // 🔥 фикс
  music.volume = vol;

  } else {
    clearInterval(fade);
  }
}, 200); // 🎵 музыка

      showWords();
    }

  }, 1000);

}, 3000);

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
  createBook();
}

document.body.addEventListener("click", () => {
  music.muted = true;
music.play().then(()=>{
  music.muted = false;
});
}, { once: true });


const heartContainer = document.getElementById("photoHeart");

function checkAllFlipped(){
  const pages = document.querySelectorAll(".page");

  const allFlipped = Array.from(pages).every(p =>
    p.classList.contains("flipped")
  );

  console.log("pages:", pages.length, "allFlipped:", allFlipped);

  if(allFlipped){
    setTimeout(showHeart, 800);
  }
}

function showHeart(){
  album.style.display = "none";
  heartContainer.style.display = "block";

  heartContainer.innerHTML = "";

  const positions = [];

  for(let t = 0; t < Math.PI * 2; t += 0.3){
    const x = 16 * Math.pow(Math.sin(t),3);
    const y = 13 * Math.cos(t) - 5 * Math.cos(2*t) - 2 * Math.cos(3*t) - Math.cos(4*t);

    positions.push({
      x: x * 10 + 200,
      y: -y * 10 + 200
    });
  }

  let completed = 0;

  positions.forEach((pos, i)=>{
    const img = document.createElement("img");
    img.src = photos[i % photos.length];

    img.style.left = "200px";
    img.style.top = "200px";

    heartContainer.appendChild(img);

    setTimeout(()=>{
      img.style.left = pos.x + "px";
      img.style.top = pos.y + "px";

      completed++;

      if(completed === positions.length){
        setTimeout(()=>{
          const finalText = document.getElementById("finalText");
          finalText.classList.add("show"); // 🔥 добавляем анимацию
        }, 800); // задержка после формирования сердца
      }

    }, 50 * i);
  });
}

setTimeout(() => {
  document.getElementById("finalText").style.display = "block";
}, positions.length * 50 + 1000);
</script>
</body>
</html>
