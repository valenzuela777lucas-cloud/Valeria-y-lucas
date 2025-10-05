<!doctype html>

<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Feliz Cumple — Portada</title>
  <style>
    :root{--bg:#0b1220;--card:#f8f5f2;--accent:#ff6b81}
    html,body{height:100%;margin:0;font-family:Inter,system-ui,Segoe UI,Roboto,"Helvetica Neue",Arial}
    body{background: radial-gradient(1200px 600px at 10% 10%, #102033 0%, #07101a 30%, var(--bg) 100%);display:flex;align-items:center;justify-content:center;color:#fff}.stage{width:min(920px,96vw);height:min(620px,86vh);position:relative;perspective:1400px}

.book{width:100%;height:100%;position:relative;transform-style:preserve-3d;}

.cover{position:absolute;inset:0;border-radius:18px;display:flex;flex-direction:column;align-items:center;justify-content:center;box-shadow:0 20px 60px rgba(3,8,15,.6);cursor:pointer;backface-visibility:hidden;transform-origin:left;background:linear-gradient(180deg,#1b2a47 0%, #122033 100%);transition:transform .9s cubic-bezier(.2,.9,.2,1);overflow:hidden;padding:28px;gap:16px}

.cover.open{transform:rotateY(-160deg) translateX(-6%);box-shadow:0 30px 80px rgba(2,6,12,.75)}

.title{font-size:clamp(30px,4.5vw,48px);font-weight:700;letter-spacing:.8px;color:var(--card);animation:shine 3s infinite alternate}
@keyframes shine{0%{text-shadow:0 0 8px rgba(255,255,255,0.2);}100%{text-shadow:0 0 20px rgba(255,255,255,0.8),0 0 40px var(--accent);}}

.subtitle{opacity:.9;font-size:clamp(14px,1.6vw,18px);margin-top:6px}

.book-edge{position:absolute;inset:0;border-radius:18px;background:linear-gradient(90deg, rgba(255,255,255,0.03), rgba(255,255,255,0.01) 60%);pointer-events:none}

.press{margin-top:18px;padding:8px 14px;border-radius:999px;background:rgba(255,255,255,0.06);font-weight:600;letter-spacing:.6px}

.page{position:absolute;left:0;top:0;right:0;bottom:0;border-radius:18px;background:linear-gradient(180deg,#fff 0%, #fff 100%);color:#0b1220;padding:36px;transform:translateX(2%) scale(.99);box-shadow:inset 0 0 0 1px rgba(6,10,20,.03);opacity:0;pointer-events:none;transition:opacity .6s ease .2s, transform .7s cubic-bezier(.2,.9,.2,1) .1s}
.page.show{opacity:1;pointer-events:auto;transform:translateX(0) scale(1)}

.letter-title{font-size:clamp(20px,2.4vw,30px);font-weight:700;margin-bottom:6px}
.letter{line-height:1.6;font-size:clamp(15px,1.6vw,18px);max-width:72ch}

.celebrate{position:absolute;bottom:22px;right:22px;background:linear-gradient(90deg,var(--accent),#ffb86b);color:#fff;padding:12px 16px;border-radius:12px;font-weight:700;cursor:pointer;border:none}

#fireworks{position:absolute;inset:0;pointer-events:none}

.balloon{position:absolute;width:54px;height:74px;border-radius:50% 50% 46% 46%;display:flex;align-items:center;justify-content:center;transform-origin:center bottom;}
.string{position:absolute;bottom:-66px;left:50%;width:2px;height:66px;background:rgba(255,255,255,0.18);border-radius:2px;transform:translateX(-50%)}

@keyframes floatUp{0%{transform:translateY(30vh) scale(1) rotate(-6deg)}50%{transform:translateY(10vh) scale(1.05) rotate(6deg)}100%{transform:translateY(-40vh) scale(1) rotate(-6deg)}}

.hint{position:absolute;left:18px;bottom:18px;font-size:12px;opacity:.7}
@media (max-width:520px){.stage{height:84vh}}

  </style>
</head>
<body>
  <div class="stage">
    <div class="book">
      <div id="cover" class="cover" tabindex="0" aria-pressed="false">
        <div class="book-edge"></div>
        <div style="text-align:center">
          <div class="title">💖 VALERIA Y LUCAS 💖</div>
          <div class="subtitle">Un amor que escribe su propia historia</div>
          <div class="press">Toca para abrir ▶</div>
        </div>
      </div><div id="page" class="page" aria-hidden="true">
    <div class="letter-title">Para mi amor,</div>
    <div id="letter" class="letter"></div>
    <button id="celebrateBtn" class="celebrate">¡Celebrar! 🎉</button>
  </div>

  <canvas id="fireworks"></canvas>
</div>
<div class="hint">Diseñado con cariño — abre y escucha ♥</div>

  </div>  <script>
    const carta = `Feliz cumpleaños, mi amor.

Hoy celebramos el regalo más bonito que la vida me dio: tú. Gracias por cada sonrisa, por tu paciencia y por cada momento junto a ti. Que este año te llene de sueños cumplidos y momentos inolvidables. Te amo más cada día.

Mi amor, quiero que sepas que cada día a tu lado me confirma que eres la persona con la que quiero compartir mi vida. Sueño con un futuro juntos, con una familia, con risas, abrazos y días llenos de amor. No hay nadie más con quien quiera construir ese hermoso destino que contigo. Te amo más de lo que las palabras pueden decir.

Con todo mi cariño, tu Lucas.`;

    const cover=document.getElementById('cover');
    const page=document.getElementById('page');
    const letterEl=document.getElementById('letter');
    const fireworksCanvas=document.getElementById('fireworks');
    const celebrateBtn=document.getElementById('celebrateBtn');

    function resizeCanvas(){fireworksCanvas.width=window.innerWidth;fireworksCanvas.height=window.innerHeight;}
    window.addEventListener('resize',resizeCanvas);resizeCanvas();

    function typeText(el,text,speed=24){el.textContent='';let i=0;const interval=setInterval(()=>{el.textContent+=text.charAt(i);i++;if(i>=text.length)clearInterval(interval);},speed);}

    function speak(text){if(!('speechSynthesis'in window))return;const u=new SpeechSynthesisUtterance(text);u.lang='es-ES';u.rate=1;u.pitch=1;window.speechSynthesis.cancel();window.speechSynthesis.speak(u);}

    function spawnBalloons(count=9){const colors=['#ff6b81','#ffb86b','#7be495','#6ac7ff','#b28cff','#ffd1dc'];for(let i=0;i<count;i++){const b=document.createElement('div');b.className='balloon';const w=44+Math.round(Math.random()*32);b.style.width=w+'px';b.style.height=Math.round(w*1.35)+'px';const color=colors[Math.floor(Math.random()*colors.length)];b.style.background=`radial-gradient(circle at 30% 25%, rgba(255,255,255,0.7), rgba(255,255,255,0.05) 20%), ${color}`;const left=Math.random()*80;b.style.left=left+'vw';b.style.bottom='-10vh';b.style.animation=`floatUp ${8+Math.random()*8}s ease-in forwards`;b.style.animationDelay=(Math.random()*2)+'s';const s=document.createElement('div');s.className='string';b.appendChild(s);document.body.appendChild(b);setTimeout(()=>b.remove(),20000);}}

    (function(){const ctx=fireworksCanvas.getContext('2d');let particles=[];function rand(min,max){return Math.random()*(max-min)+min}function Particle(x,y,dx,dy,color){this.x=x;this.y=y;this.vx=dx;this.vy=dy;this.alpha=1;this.color=color;this.size=rand(2,4)}Particle.prototype.update=function(dt){this.vy+=0.06;this.x+=this.vx*dt;this.y+=this.vy*dt;this.alpha-=0.009;}Particle.prototype.draw=function(ctx){ctx.globalAlpha=Math.max(0,this.alpha);ctx.fillStyle=this.color;ctx.beginPath();ctx.arc(this.x,this.y,this.size,0,Math.PI*2);ctx.fill();}function explode(x,y){const hue=Math.floor(rand(0,360));for(let i=0;i<80;i++){const angle=Math.random()*Math.PI*2;const speed=Math.random()*6+1;const dx=Math.cos(angle)*speed;const dy=Math.sin(angle)*speed;const color=`hsl(${hue} ${Math.floor(rand(60,95))}% ${Math.floor(rand(45,60))}%)`;particles.push(new Particle(x,y,dx,dy,color))}}let last=performance.now();function frame(now){const dt=Math.min(1,(now-last)/16);last=now;ctx.clearRect(0,0,fireworksCanvas.width,fireworksCanvas.height);for(let i=particles.length-1;i>=0;i--){const p=particles[i];p.update(dt);p.draw(ctx);if(p.alpha<=0)particles.splice(i,1)}requestAnimationFrame(frame);}frame(performance.now());window.launchFirework=function(){const x=rand(100,fireworksCanvas.width-100);const y=rand(100,fireworksCanvas.height/2);explode(x,y);}})();

    function openCover(){if(cover.classList.contains('open'))return;cover.classList.add('open');cover.setAttribute('aria-pressed','true');setTimeout(()=>{page.classList.add('show');page.setAttribute('aria-hidden','false');typeText(letterEl,carta,26);speak('Feliz cumpleaños, mi amor. Te amo.');for(let i=0;i<6;i++){setTimeout(()=>window.launchFirework(),400*i)}spawnBalloons(9);},700);}

    cover.addEventListener('click',openCover);cover.addEventListener('keydown',e=>{if(e.key==='Enter'||e.key===' ')openCover();});

    celebrateBtn.addEventListener('click',()=>{for(let i=0;i<14;i++){setTimeout(()=>window.launchFirework(),i*150)}spawnBalloons(16);speak('Que tengas un cumpleaños maravilloso. Te amo.');});
  </script></body>
</html>
