<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>For Riya 💖</title>
<link href="https://fonts.googleapis.com/css2?family=Great+Vibes&family=Poppins:wght@300;500&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<style>
/* ===== BACKGROUND ===== */
body{
  margin:0;font-family:Poppins,sans-serif;color:#5a1a2f;overflow-x:hidden;
  background:
    radial-gradient(circle at 20px 20px, rgba(255,182,193,.6) 6px, transparent 7px),
    radial-gradient(circle at 60px 60px, rgba(255,105,180,.5) 6px, transparent 7px),
    repeating-linear-gradient(45deg, rgba(255,0,0,.25) 0, rgba(255,0,0,.25) 10px, transparent 10px, transparent 20px),
    repeating-linear-gradient(-45deg, rgba(255,105,180,.25) 0, rgba(255,105,180,.25) 10px, transparent 10px, transparent 20px),
    #fff0f5;
  background-size:80px 80px, 80px 80px, auto, auto, auto;
}
.container{max-width:900px;margin:auto;padding:20px;text-align:center}
h1{font-family:"Great Vibes",cursive;font-size:3rem}

/* ===== LOCK ===== */
.lock{position:fixed;inset:0;background:rgba(255,230,236,.96);display:flex;flex-direction:column;justify-content:center;align-items:center;z-index:999;padding:20px}
.pw-wrap{display:flex;align-items:center;gap:8px}
.lock input{padding:10px 12px;border-radius:10px;border:none;margin:6px;text-align:center;letter-spacing:4px;font-size:1.1rem;width:180px}
.eye{cursor:pointer;font-size:1.2rem;user-select:none}
.keyboard{display:grid;grid-template-columns:repeat(7,1fr);gap:8px;max-width:320px;margin:10px auto}
.key{padding:12px 0;border-radius:10px;background:#fff;border:1px solid #ffd1dc;cursor:pointer;font-weight:700}
.key.used{opacity:.4}
.key.hint{outline:3px dashed #ff4d6d}
@keyframes shake{25%{transform:translateX(-3px)}50%{transform:translateX(3px)}}
.key.shake{animation:shake .15s}
.lock .actions{display:flex;gap:8px}
.lock button{padding:10px 14px;border:none;border-radius:10px;background:#ff4d6d;color:#fff}

/* ===== ENVELOPE + TEDDY ===== */
.envelope-wrap{position:relative;width:320px;height:220px;margin:30px auto;perspective:900px}
.envelope{position:relative;width:100%;height:100%;background:#f8f2f4;border-radius:12px;box-shadow:0 10px 25px rgba(0,0,0,.2);overflow:hidden}
.flap{position:absolute;top:0;left:0;width:100%;height:50%;background:linear-gradient(135deg,#ffd1dc,#fff);transform-origin:top;transition:.5s;clip-path:polygon(0 0,100% 0,50% 100%)}
.envelope.open .flap{transform:rotateX(160deg)}
.letter{position:absolute;left:10px;right:10px;bottom:10px;height:140px;background:#fff;border-radius:8px;padding:10px;font-family:"Great Vibes",cursive;font-size:1.5rem;transform:translateY(100%);transition:.5s;opacity:0}
.envelope.open .letter{transform:translateY(0);opacity:1}
.teddy{position:absolute;left:50%;top:55%;transform:translate(-50%,-50%);font-size:72px;cursor:pointer;user-select:none;z-index:2;filter:drop-shadow(0 6px 6px rgba(0,0,0,.2));animation:float 2.5s ease-in-out infinite}
.teddy.hide{opacity:0;pointer-events:none}
.teddy.beat{animation:float 2.5s ease-in-out infinite, beat .8s ease-in-out infinite}
@keyframes float{50%{transform:translate(-50%,-58%)}}
@keyframes beat{50%{transform:translate(-50%,-50%) scale(1.06)}}

/* ===== OTHER UI ===== */
.controls input,.controls button,.controls select{padding:8px;margin:6px;border-radius:6px;border:none}
.gallery{display:grid;grid-template-columns:repeat(auto-fit,minmax(100px,1fr));gap:10px;padding:15px;border:8px solid pink;border-radius:20px;background:rgba(255,255,255,.7)}
.gallery img{width:100%;border-radius:12px;object-fit:cover}
#map{height:300px;border-radius:12px;margin:15px 0}
.heart{position:fixed;top:-10px;animation:fall linear infinite;font-size:18px}
@keyframes fall{to{transform:translateY(100vh)}}
.firework,.confetti{position:fixed;width:8px;height:8px;border-radius:50%;animation:explode 1s ease-out forwards}
@keyframes explode{to{transform:translate(var(--x),var(--y)) scale(0);opacity:0}}
.countdown{font-size:1.2rem;margin:10px 0}
.poem-box{margin:10px auto;padding:12px;border-radius:12px;background:rgba(255,255,255,.85);max-width:500px;font-family:"Great Vibes",cursive;font-size:1.4rem;display:none}
.wax{display:flex;gap:8px;justify-content:center;margin:10px}
.wax span{font-size:28px;cursor:pointer}
.wax .on{filter:drop-shadow(0 0 6px #ff4d6d)}
.toast{position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:#ff4d6d;color:#fff;padding:10px 14px;border-radius:999px;display:none}
</style>
</head>
<body>

<div class="lock" id="lock">
  <h2>🔐 Enter password to unlock</h2>
  <div class="pw-wrap">
    <input id="pw" type="password" placeholder="••••••" readonly>
    <span class="eye" id="eye">👁️</span>
  </div>
  <div class="keyboard" id="keyboard"></div>
  <div class="actions">
    <button onclick="hint()">Hint 👀</button>
    <button onclick="resetKeys()">Reset</button>
  </div>
  <p id="err" style="color:red"></p>
</div>

<div class="container" id="main" style="display:none">
  <h1 id="welcome">For Riya 💕</h1>
  <div class="countdown" id="countdown"></div>

  <div class="wax">
    <span onclick="seal(this)">❤️</span>
    <span onclick="seal(this)">🌹</span>
    <span onclick="seal(this)">💌</span>
    <span onclick="seal(this)">⭐</span>
  </div>

  <div class="controls">
    <input id="msg" placeholder="Type your love message for Riya 💌">
    <input type="file" multiple accept="audio/*" onchange="uploadVoices(event)">
    <select id="border" onchange="border()">
      <option value="pink">🌸 Pink Floral</option>
      <option value="gold">🌼 Gold Floral</option>
      <option value="violet">🌷 Violet Floral</option>
    </select>
    <button onclick="poem()">Generate Secret Poem ✨</button>
    <button onclick="music()">🎵 Music On / Off</button>
    <button onclick="shot()">📸 Save this moment</button>
  </div>

  <div class="poem-box" id="poemBox"></div>

  <div class="envelope-wrap" id="shotArea">
    <div class="envelope" id="env">
      <div class="flap"></div>
      <div class="letter" id="letter">Riya, my favorite maldita, loving you is the easiest thing in my life. 💗</div>
    </div>
    <div class="teddy" id="teddy">🧸</div>
  </div>

  <h2>Our Memories 📸</h2>
  <input type="file" multiple accept="image/*" onchange="photos(event)">
  <div class="gallery" id="gallery"></div>

  <h2>Our Favorite Places 🗺️</h2>
  <div id="map"></div>
</div>

<div class="toast" id="toast">Saved 📸</div>

<audio id="bgm" loop preload="auto">
  <source src="https://cdn.pixabay.com/download/audio/2023/05/22/audio_8f1b7a0c6a.mp3?filename=love-story-146239.mp3" type="audio/mpeg">
</audio>
<audio id="voice" preload="auto"></audio>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
/* ===== SHAREABLE LINK AUTO-UNLOCK ===== */
const params=new URLSearchParams(location.search);
if(params.get("code")==="mahalko"){ setTimeout(unlock, 300); }

/* ===== PASSWORD PUZZLE ===== */
const PASSWORD="mahalko";
const pwInput=document.getElementById("pw");
const eye=document.getElementById("eye");
const keyboard=document.getElementById("keyboard");
let picked=[], showLetters=false;

function shuffle(a){ for(let i=a.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[a[i],a[j]]=[a[j],a[i]];} return a;}
const decoys=["x","z","q","p","y","t","e"];
const letters=shuffle(PASSWORD.split("").concat(decoys));

letters.forEach(l=>{
  const b=document.createElement("button");
  b.className="key";
  b.textContent=l;
  b.onclick=()=>{
    if(navigator.vibrate) navigator.vibrate(20);
    b.classList.add("shake");
    setTimeout(()=>b.classList.remove("shake"),150);

    if(PASSWORD.includes(l)){
      picked.push(l);
      renderPw();
      b.classList.add("used"); b.disabled=true;
      if(picked.sort().join("")===PASSWORD.split("").sort().join("")) unlock();
    } else {
      document.getElementById("err").innerText="Oops, not part of the password 😅";
      if(navigator.vibrate) navigator.vibrate([60,40,60]);
    }
  };
  keyboard.appendChild(b);
});

function renderPw(){
  if(showLetters){ pwInput.type="text"; pwInput.value=picked.join(""); }
  else{ pwInput.type="password"; pwInput.value="•".repeat(picked.length); }
}
eye.onclick=()=>{ showLetters=!showLetters; eye.textContent=showLetters?"🙈":"👁️"; renderPw(); };
function resetKeys(){ location.reload(); }
function hint(){
  const remaining=PASSWORD.split("").filter(l=>!picked.includes(l));
  const k=[...document.querySelectorAll(".key")].find(b=>remaining.includes(b.textContent) && !b.disabled);
  if(k){ k.classList.add("hint"); setTimeout(()=>k.classList.remove("hint"),1500); }
}

/* ===== UNLOCK ===== */
function unlock(){
  document.getElementById("lock").style.display="none";
  document.getElementById("main").style.display="block";
  document.getElementById("welcome").innerText="Welcome, Riya 💖";
  celebrate();
  bgm.play().catch(()=>{});
}

/* ===== ENVELOPE + TEDDY ===== */
const env=document.getElementById("env"), letter=document.getElementById("letter");
const bgm=document.getElementById("bgm"), voice=document.getElementById("voice");
const teddy=document.getElementById("teddy");
let voices=[];

function showLetter(){
  teddy.classList.add("hide","beat");
  env.classList.add("open");
  fireworks();
  if(voices.length){
    const v=voices[Math.floor(Math.random()*voices.length)];
    voice.src=v; voice.currentTime=0; voice.play().catch(()=>{});
  }
}
function hideLetter(){ teddy.classList.remove("hide","beat"); env.classList.remove("open"); }

teddy.addEventListener("touchstart", showLetter);
teddy.addEventListener("mousedown", showLetter);
teddy.addEventListener("touchend", hideLetter);
teddy.addEventListener("mouseup", hideLetter);
teddy.addEventListener("mouseleave", hideLetter);

function music(){ bgm.paused?bgm.play().catch(()=>{}):bgm.pause(); }
function uploadVoices(e){ voices=[...e.target.files].map(f=>URL.createObjectURL(f)); toast("Voices ready 🎙️"); }

/* ===== WAX SEAL ===== */
function seal(el){
  document.querySelectorAll(".wax span").forEach(s=>s.classList.remove("on"));
  el.classList.add("on");
  toast("Seal selected "+el.textContent);
}

/* ===== POEM ===== */
function poem(){
  const p=[
    "Every heartbeat carries your name,\nEvery sunset whispers our flame.",
    "In a noisy world, you are my calm,\nMy forever safe and gentle charm.",
    "If love were a road, I’d walk it twice,\nJust to hold your hand in every life."
  ];
  const box=document.getElementById("poemBox");
  box.style.display="block";
  box.innerText=p[Math.floor(Math.random()*p.length)];
}

/* ===== HEARTS ===== */
setInterval(()=>{const h=document.createElement("div");h.className="heart";h.innerText="💗";h.style.left=Math.random()*100+"vw";h.style.animationDuration=(3+Math.random()*5)+"s";document.body.appendChild(h);setTimeout(()=>h.remove(),8000)},400);

/* ===== FIREWORKS + CONFETTI ===== */
function fireworks(){
  for(let i=0;i<25;i++) boom("firework");
}
function celebrate(){
  for(let i=0;i<40;i++) boom("confetti");
}
function boom(cls){
  const f=document.createElement("div");
  f.className=cls;
  f.style.left=(50+Math.random()*20-10)+"%";
  f.style.top=(40+Math.random()*20-10)+"%";
  f.style.background=["#ff4d6d","#ffd166","#cdb4db","#ffafcc"][Math.floor(Math.random()*4)];
  f.style.setProperty("--x",(Math.random()*400-200)+"px");
  f.style.setProperty("--y",(Math.random()*400-200)+"px");
  document.body.appendChild(f);
  setTimeout(()=>f.remove(),1000);
}

/* ===== COUNTDOWN ===== */
function countdown(){
  const v=new Date(new Date().getFullYear(),1,14), n=new Date(), d=v-n;
  document.getElementById("countdown").innerText=d>0?`⏳ ${Math.floor(d/86400000)} days left until Valentine’s Day`:"💘 Happy Valentine’s Day, Riya!";
}
setInterval(countdown,1000); countdown();

/* ===== MAP WITH NOTES ===== */
const map=L.map('map').setView([14.5995,120.9842],6);
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
map.on('click',e=>{
  const note=prompt("Memory note for this place:");
  if(note) L.marker(e.latlng).addTo(map).bindPopup(note).openPopup();
});

/* ===== GALLERY ===== */
function photos(e){
  [...e.target.files].forEach(f=>{
    const i=document.createElement("img");
    i.src=URL.createObjectURL(f); gallery.appendChild(i);
  });
}
function border(){ gallery.style.borderColor=document.getElementById("border").value; }

/* ===== SCREENSHOT (simple) ===== */
function shot(){
  alert("Tip: Use your phone's screenshot for best quality 📸");
  toast("Captured moment 💖");
}

/* ===== TOAST ===== */
function toast(t){
  const el=document.getElementById("toast");
  el.innerText=t; el.style.display="block";
  setTimeout(()=>el.style.display="none",1500);
}
</script>
</body>
</html>
