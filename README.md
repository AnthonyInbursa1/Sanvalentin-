
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Para Ti ❤️</title>

<style>
body{
  margin:0;height:100vh;overflow:hidden;
  background:radial-gradient(circle at top,#3a0015,#000);
  font-family:'Segoe UI',sans-serif;
}
canvas{position:fixed;top:0;left:0;pointer-events:none;}

.mensaje,.pregunta,.final,.perrito{
  position:absolute;width:100%;text-align:center;z-index:10;
  color:#fff;text-shadow:0 0 20px #ff4d6d;
}
.mensaje{top:6%;font-size:clamp(1.5em,4vw,2.2em);}
.pregunta{bottom:32%;font-size:clamp(2em,6vw,2.8em);color:#ff4d6d;}

.botones{position:absolute;bottom:18%;width:100%;text-align:center;z-index:10;}

button{
  font-size:1.2em;padding:14px 32px;
  border-radius:40px;border:none;cursor:pointer;
}

.si{background:#ff4d6d;color:#fff;box-shadow:0 0 20px #ff4d6d;}
.no{
  background:#555;color:#fff;
  position:absolute;
}

.final{
  top:35%;
  font-size:clamp(2em,6vw,2.6em);
  display:none;
}

.perrito{
  top:45%;
  font-size:4em;
  display:none;
  animation:brincar 1s infinite;
}

@keyframes brincar{
  0%{transform:translateY(0);}
  50%{transform:translateY(-25px);}
  100%{transform:translateY(0);}
}

.rosa{
  position:absolute;width:90px;height:90px;
  background:url('https://i.imgur.com/0Z8Fz3G.png') center/contain no-repeat;
  animation:crecer 6s ease forwards;
}

@keyframes crecer{
  from{transform:scale(0);opacity:0;}
  to{transform:scale(1) rotate(360deg);opacity:1;}
}
</style>
</head>

<body>

<canvas id="fireworks"></canvas>

<div class="mensaje" id="mensajeTop"></div>
<div class="pregunta">¿Quieres ser mi San Valentín? ❤️</div>

<div class="botones">
  <button class="si" onclick="respuestaSi()">Sí 💖</button>
  <button class="no" id="btnNo" onclick="mordida()">No 🙈</button>
</div>

<div class="final" id="final">
MI AMOR ❤️💘<br>
Sabía que dirías que sí 🌹
</div>

<div class="perrito" id="perrito">
🐶<br>JAJAJA te a mordido un perro
</div>

<audio id="musica" loop>
  <source src="cancion.mp3" type="audio/mpeg">
</audio>

<script>
// ✏️ NOMBRE
const nombreElla = "MI AMOR ❤️";
document.getElementById("mensajeTop").innerHTML =
`Para ${nombreElla}, la persona que hace florecer mi corazón 🌹`;

// 🌹 ROSAS
let rosas = setInterval(()=>{
  const r=document.createElement("div");
  r.className="rosa";
  r.style.left=Math.random()*innerWidth+"px";
  r.style.top=Math.random()*innerHeight+"px";
  document.body.appendChild(r);
  setTimeout(()=>r.remove(),7000);
},700);

// 😈 BOTÓN NO IMPOSIBLE
const btnNo=document.getElementById("btnNo");
setInterval(()=>{
  btnNo.style.left=Math.random()*(innerWidth-120)+"px";
  btnNo.style.top=Math.random()*(innerHeight-120)+"px";
},350);

// 🐶 SI LOGRA DARLE
function mordida(){
  const p=document.getElementById("perrito");
  p.style.display="block";
  setTimeout(()=>p.style.display="none",3000);
}

// ❤️ FUEGOS CORAZÓN
const canvas=document.getElementById("fireworks");
const ctx=canvas.getContext("2d");
canvas.width=innerWidth;canvas.height=innerHeight;
let particles=[], activo=false;

function crearCorazon(){
  const cx=canvas.width/2, cy=canvas.height/2;
  for(let t=0;t<Math.PI*2;t+=0.1){
    const x=16*Math.pow(Math.sin(t),3);
    const y=-(13*Math.cos(t)-5*Math.cos(2*t)-2*Math.cos(3*t)-Math.cos(4*t));
    particles.push({x:cx,y:cy,dx:x,dy:y,a:1});
  }
}

function animar(){
  if(!activo) return;
  ctx.clearRect(0,0,canvas.width,canvas.height);
  particles.forEach(p=>{
    ctx.fillStyle=`rgba(255,80,120,${p.a})`;
    ctx.beginPath();ctx.arc(p.x,p.y,2,0,Math.PI*2);ctx.fill();
    p.x+=p.dx*0.05;p.y+=p.dy*0.05;p.a-=0.006;
  });
  particles=particles.filter(p=>p.a>0);
  requestAnimationFrame(animar);
}

// 💖 SÍ
function respuestaSi(){
  clearInterval(rosas);
  document.querySelector(".pregunta").style.display="none";
  document.querySelector(".botones").style.display="none";
  document.getElementById("final").style.display="block";

  document.getElementById("musica").play();
  activo=true;
  crearCorazon();
  animar();
}
</script>

</body>
</html>
