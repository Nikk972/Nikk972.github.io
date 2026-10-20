# Il mio codice di prova

Ecco un esempio di codice HTML che ho scritto:

```<!DOCTYPE html>
<html lang="it" class="h-full">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Sanctum Chess – Cattedrale Gotica</title>
<meta name="description" content="Scacchi 3D in una cattedrale gotica: gioca contro un amico o contro l'IA, da PC o da telefono.">
<meta name="theme-color" content="#040306">
<meta name="color-scheme" content="dark">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="Sanctum Chess">
<meta property="og:title" content="Sanctum Chess – Cattedrale Gotica">
<meta property="og:description" content="Scacchi 3D in una cattedrale gotica, giocabili da PC e da telefono.">
<meta property="og:type" content="website">
<meta property="og:url" content="https://nikk972.github.io/">
<link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Crect width='100' height='100' rx='18' fill='%23040306'/%3E%3Ctext x='50' y='74' font-size='72' text-anchor='middle' fill='%23b08a4a'%3E%E2%99%9A%3C/text%3E%3C/svg%3E">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;600;700;900&family=Cormorant+Garamond:ital,wght@0,400;0,600;1,400&display=swap" rel="stylesheet">
<!-- Librerie da CDN, con CDN di riserva se il primo non risponde -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>window.THREE || document.write('<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/build/three.min.js"><\/script>')</script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
<script>(window.THREE && THREE.OrbitControls) || document.write('<script src="https://unpkg.com/three@0.128.0/examples/js/controls/OrbitControls.js"><\/script>')</script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/chess.js/0.10.3/chess.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/shaders/CopyShader.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/shaders/LuminosityHighPassShader.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/postprocessing/EffectComposer.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/postprocessing/RenderPass.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/postprocessing/ShaderPass.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/postprocessing/UnrealBloomPass.js"></script>
<script>window.Chess || document.write('<script src="https://cdn.jsdelivr.net/npm/chess.js@0.10.3/chess.min.js"><\/script>')</script>
<style>
:root{--ink:#d8ccb6;--muted:#9a8b72;--gold:#c9a35f;--line:rgba(201,163,95,.28);--panel:rgba(8,6,10,.94);--green:#7fb88d;--red:#e0584a}
*{box-sizing:border-box}
html,body{margin:0;height:100%;height:100dvh;background:#020203;color:var(--ink);overflow:hidden;font-family:'Cormorant Garamond',Georgia,'Times New Roman',serif;-webkit-tap-highlight-color:transparent;overscroll-behavior:none;user-select:none;-webkit-user-select:none}
h1,h2,h3,button,select,.cinzel{font-family:'Cinzel',Georgia,serif}
button{cursor:pointer;color:inherit}
.hidden{display:none!important}
#canvas-container{position:fixed;inset:0;touch-action:none;cursor:grab}
#canvas-container:active{cursor:grabbing}
#canvas-container canvas{display:block}
/* barra di stato */
.statusbar{position:fixed;left:0;right:0;bottom:0;z-index:20;display:flex;align-items:center;gap:.75rem;
  padding:.45rem max(.75rem,env(safe-area-inset-right)) calc(.45rem + env(safe-area-inset-bottom)) max(.75rem,env(safe-area-inset-left));
  background:linear-gradient(to top,rgba(4,3,6,.94),rgba(4,3,6,.72));border-top:1px solid var(--line);backdrop-filter:blur(6px);-webkit-backdrop-filter:blur(6px);transition:box-shadow .3s,border-color .3s}
.statusbar.in-check{box-shadow:0 -6px 26px rgba(200,40,40,.35);border-top-color:rgba(224,88,74,.7)}
.sb-left{display:flex;align-items:center;gap:1rem;flex:1;min-width:0;font-size:1.08rem;white-space:nowrap;overflow:hidden}
.dot{width:.85rem;height:.85rem;border-radius:50%;border:1px solid var(--gold);flex:none;transition:background .3s}
.dot.w{background:#ece4d2}.dot.b{background:#17131a}
.sb-turn strong{font-family:'Cinzel',serif;color:var(--gold);font-weight:600}
.sb-turn strong.mate{color:var(--red)}.sb-turn strong.draw{color:#e0c27a}
.lbl{color:var(--muted)}
.sb-check.ok{color:var(--green)}.sb-check.alert{color:var(--red);font-weight:600;animation:pulse 1.2s infinite}
@keyframes pulse{50%{opacity:.45}}
.sb-last{color:var(--muted);overflow:hidden;text-overflow:ellipsis}
.sb-timers{display:flex;gap:.7rem;font-variant-numeric:tabular-nums}
.sb-timers b{font-weight:600;color:var(--muted)}.sb-timers b.run{color:var(--gold)}
.sb-menu{flex:none;width:2.7rem;height:2.4rem;border-radius:.45rem;background:#140f0a;border:1px solid #5c4a30;font-size:1.2rem;line-height:1}
.sb-menu:hover{border-color:var(--gold)}
@media (max-width:560px){.lbl,.sb-last{display:none}.sb-left{gap:.65rem;font-size:1rem}}
/* menu laterale / foglio dal basso */
#scrim{position:fixed;inset:0;z-index:30;background:rgba(0,0,0,.35)}
#drawer{position:fixed;z-index:40;top:0;right:0;bottom:0;width:min(380px,92vw);background:var(--panel);border-left:1px solid var(--line);
  display:flex;flex-direction:column;transform:translateX(105%);transition:transform .3s ease;visibility:hidden;
  padding-top:env(safe-area-inset-top);padding-bottom:env(safe-area-inset-bottom);backdrop-filter:blur(8px);-webkit-backdrop-filter:blur(8px)}
#drawer.open{transform:none;visibility:visible}
@media (max-width:640px){#drawer{top:auto;left:0;width:auto;max-height:80dvh;border-left:0;border-top:1px solid var(--line);border-radius:14px 14px 0 0;transform:translateY(105%)}}
.dr-head{display:flex;justify-content:space-between;align-items:center;padding:.9rem 1.1rem .5rem}
.dr-head h2{margin:0;font-size:1.05rem;color:var(--gold);letter-spacing:.06em}
.x{background:none;border:0;font-size:1.3rem;color:var(--gold);padding:.2rem .4rem}
.tabs{display:flex;overflow-x:auto;border-bottom:1px solid var(--line);padding:0 .6rem;scrollbar-width:none;flex:none}
.tabs::-webkit-scrollbar{display:none}
.tab-btn{background:none;border:0;border-bottom:2px solid transparent;padding:.6rem .55rem;font-size:.72rem;color:var(--muted);white-space:nowrap}
.tab-btn.active{color:#efe2c4;border-color:var(--gold)}
.panels{flex:1;overflow-y:auto;padding:.4rem 1.1rem 1rem;-webkit-overflow-scrolling:touch;overscroll-behavior:contain}
.sec{font-family:'Cinzel',serif;font-size:.72rem;color:var(--gold);margin:1rem 0 .45rem;letter-spacing:.05em}
.seg{display:flex;flex-wrap:wrap;gap:.35rem}
.gbtn{background:linear-gradient(160deg,#120d09,#21180f);border:1px solid #5c4a30;border-radius:.35rem;padding:.5rem .75rem;font-size:.74rem;transition:border-color .2s,background .2s}
.gbtn:hover{border-color:#a8854f}.gbtn.active{background:linear-gradient(160deg,#3a2a16,#5c4426);border-color:var(--gold);color:#fff}
.gbtn:focus-visible,.tab-btn:focus-visible,.sb-menu:focus-visible,select:focus-visible,input:focus-visible{outline:2px solid var(--gold);outline-offset:2px}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:.35rem}.grid3{display:grid;grid-template-columns:repeat(3,1fr);gap:.35rem}
.set-row{display:flex;justify-content:space-between;align-items:center;gap:1rem;padding:.45rem 0;border-bottom:1px solid rgba(92,74,48,.25);font-size:1.02rem}
.set-row input[type=range]{width:9rem;accent-color:#a8854f}
.set-row input[type=checkbox]{width:1.15rem;height:1.15rem;accent-color:#a8854f;cursor:pointer}
.set-row select{background:#120d09;border:1px solid #5c4a30;color:var(--ink);border-radius:.3rem;padding:.25rem .4rem;font-size:.74rem;max-width:11rem}
.dr-foot{padding:.6rem 1.1rem 1rem;border-top:1px solid var(--line);flex:none;display:flex;justify-content:flex-end}
.log{font-size:1rem;max-height:none}
.log .row{display:grid;grid-template-columns:2.2rem 1fr 1fr;padding:.15rem 0;border-bottom:1px solid rgba(92,74,48,.2)}
.log .n{color:var(--muted)}
.grave{font-size:1.4rem;min-height:1.7rem;letter-spacing:.1em}
.hint{color:var(--muted);font-style:italic;font-size:.95rem;margin:.6rem 0}
/* finestre modali */
.modal{position:fixed;inset:0;z-index:50;display:flex;align-items:center;justify-content:center;background:rgba(0,0,0,.82);padding:1rem}
.card{background:#0a080c;border:1px solid #4a3a26;border-radius:.6rem;padding:1.6rem;max-width:26rem;width:100%;text-align:center;box-shadow:0 0 40px #000}
.card h2{color:var(--gold);margin:.2rem 0 .4rem;font-size:1.4rem}
.card p{margin:0 0 1.2rem;font-size:1.1rem}
.promo{display:grid;grid-template-columns:repeat(4,1fr);gap:.5rem}
.promo .gbtn{display:flex;flex-direction:column;align-items:center;gap:.3rem;padding:.8rem .2rem}.promo .gbtn span:first-child{font-size:1.9rem}
#loader{position:fixed;inset:0;z-index:60;background:#000;display:flex;flex-direction:column;align-items:center;justify-content:center;pointer-events:none;transition:opacity 1.6s ease}
#loader .cross{font-size:3rem;color:rgba(180,120,40,.8);margin-bottom:1rem}
#loader .t{font-family:'Cinzel',serif;color:rgba(200,150,70,.8);letter-spacing:.35em;font-size:.75rem}
#vignette{position:fixed;inset:0;z-index:5;pointer-events:none;background:radial-gradient(ellipse at center,rgba(0,0,0,0) 55%,rgba(0,0,0,.55) 100%)}
#fatal{position:fixed;inset:0;z-index:70;background:#000;align-items:center;justify-content:center;padding:1.5rem;text-align:center}
@media (prefers-reduced-motion:reduce){#drawer,.statusbar{transition:none}.sb-check.alert{animation:none}}
</style>
</head>
<body>

<div id="canvas-container"></div>
<div id="vignette" class="hidden"></div>

<div id="loader"><div class="cross">✠</div><div class="t">Si aprono i portali</div></div>

<!-- Unica barra visibile: stato della partita -->
<div id="bottom-bar" class="statusbar" role="status" aria-live="polite">
  <div class="sb-left">
    <span id="turn-dot" class="dot w" aria-hidden="true"></span>
    <span class="sb-turn"><span class="lbl">Tocca al</span> <strong id="game-status">Bianco</strong></span>
    <span id="check-status" class="sb-check hidden"></span>
    <span id="timer-display-container" class="sb-timers hidden"><span>B <b id="timer-white">03:00</b></span><span>N <b id="timer-black">03:00</b></span></span>
    <span id="last-move" class="sb-last"></span>
  </div>
  <button id="btn-menu" class="sb-menu" aria-label="Apri il menu" aria-expanded="false" aria-controls="drawer">☰</button>
</div>

<div id="scrim" class="hidden"></div>
<aside id="drawer" aria-label="Menu" aria-hidden="true">
  <div class="dr-head"><h2>✠ Sanctum Chess</h2><button id="btn-close-menu" class="x" aria-label="Chiudi il menu">✕</button></div>
  <nav class="tabs">
    <button class="tab-btn active" data-tab="game">Partita</button>
    <button class="tab-btn" data-tab="view">Visuale</button>
    <button class="tab-btn" data-tab="env">Atmosfera</button>
    <button class="tab-btn" data-tab="audio">Audio</button>
    <button class="tab-btn" data-tab="log">Mosse</button>
  </nav>
  <div class="panels">
    <div data-panel="game">
      <div class="sec">Azioni</div>
      <div class="grid3">
        <button id="btn-reset" class="gbtn">⟳ Nuova</button>
        <button id="btn-undo" class="gbtn">↶ Annulla</button>
        <button id="btn-hint" class="gbtn">✦ Aiuto</button>
      </div>
      <div class="sec">Avversario</div>
      <div class="seg"><button id="btn-mode-pvp" class="gbtn active">Due giocatori</button><button id="btn-mode-ai" class="gbtn">Computer</button></div>
      <div id="ai-difficulty-container" class="hidden">
        <div class="sec">Livello del computer</div>
        <div class="seg"><button id="ai-easy" class="gbtn">Facile</button><button id="ai-med" class="gbtn active">Medio</button><button id="ai-hard" class="gbtn">Difficile</button></div>
      </div>
      <div class="sec">Tempo per giocatore</div>
      <div class="seg"><button id="timer-off" class="gbtn active">Nessuno</button><button id="timer-3m" class="gbtn">3 minuti</button><button id="timer-5m" class="gbtn">5 minuti</button></div>
      <label class="set-row" style="margin-top:.8rem"><span>Mostra le mosse possibili</span><input type="checkbox" data-key="highlights"></label>
    </div>
    <div data-panel="view" class="hidden">
      <div class="grid3">
        <button id="cam-white" class="gbtn active">Bianco</button><button id="cam-black" class="gbtn">Nero</button><button id="cam-top" class="gbtn">Dall'alto</button>
        <button id="cam-nave" class="gbtn">Navata</button><button id="cam-apse" class="gbtn">Altare</button><button id="cam-cinematic" class="gbtn">Segui turno</button>
      </div>
      <p class="hint">Trascina per ruotare, pizzica o usa la rotellina per avvicinarti.</p>
      <label class="set-row"><span>Rotazione lenta automatica</span><input type="checkbox" data-key="autorotate"></label>
    </div>
    <div data-panel="env" class="hidden">
      <div class="grid3">
        <button class="gbtn theme-btn" data-theme="sanctum">Candele</button><button class="gbtn theme-btn" data-theme="moonlight">Notte e temporale</button><button class="gbtn theme-btn" data-theme="vespers">Tramonto</button>
      </div>
      <label class="set-row" style="margin-top:.8rem"><span>Monaci, coro e creature</span><input type="checkbox" data-key="figures"></label>
    </div>
    <div data-panel="audio" class="hidden">
      <div class="seg"><button id="btn-sound" class="gbtn active">Effetti attivi</button><button id="btn-ambient" class="gbtn">Organo e coro</button></div>
      <label class="set-row" style="margin-top:.8rem"><span>Volume</span><input type="range" data-key="volume" min="0" max="1" step="0.05"></label>
    </div>
    <div data-panel="log" class="hidden">
      <div id="move-log" class="log"></div>
      <div class="sec">Catturati</div>
      <div id="graveyard-white" class="grave"></div>
      <div id="graveyard-black" class="grave" style="color:#8a7f72"></div>
    </div>
  </div>
</aside>

<div id="promotion-modal" class="modal hidden">
  <div class="card">
    <h2>Ascesa del pedone</h2><p>Scegli in cosa promuovere il pedone</p>
    <div class="promo">
      <button data-piece="q" class="promo-btn gbtn"><span>♛</span><span>Regina</span></button>
      <button data-piece="r" class="promo-btn gbtn"><span>♜</span><span>Torre</span></button>
      <button data-piece="b" class="promo-btn gbtn"><span>♝</span><span>Alfiere</span></button>
      <button data-piece="n" class="promo-btn gbtn"><span>♞</span><span>Cavallo</span></button>
    </div>
  </div>
</div>

<div id="gameover-modal" class="modal hidden">
  <div class="card">
    <div style="font-size:2.4rem;color:#b07a30">✠</div>
    <h2 id="gameover-title">Scacco matto!</h2><p id="gameover-desc">Il santuario ha decretato il vincitore.</p>
    <div class="grid2"><button id="btn-play-again" class="gbtn active">Gioca di nuovo</button><button id="btn-see-board" class="gbtn">Guarda la scacchiera</button></div>
  </div>
</div>

<div id="fatal" class="hidden">
  <div style="max-width:22rem"><div style="font-size:2.4rem;color:#b07a30">✠</div><p id="fatal-msg" style="font-size:1.15rem"></p>
  <button onclick="location.reload()" class="gbtn">Ricarica la pagina</button></div>
</div>
<script>
function showFatal(msg) { const f = document.getElementById('fatal'); document.getElementById('fatal-msg').textContent = msg; f.classList.remove('hidden'); f.style.display = 'flex'; const l = document.getElementById('loader'); if (l) l.remove(); }
(function () {
  if (typeof THREE === 'undefined' || typeof Chess === 'undefined') { showFatal('Non riesco a caricare le librerie del gioco. Controlla la connessione e ricarica la pagina.'); return; }
  const c = document.createElement('canvas'); if (!(c.getContext('webgl2') || c.getContext('webgl'))) showFatal('Questo browser o dispositivo non supporta la grafica 3D (WebGL). Prova con Chrome, Safari o Firefox aggiornati.');
})();
window.addEventListener('error', e => { if (document.getElementById('loader')) showFatal('Si è verificato un errore durante il caricamento della cattedrale. Ricarica la pagina.'); });
</script>
<script>
// =====================================================================
// IMPOSTAZIONI
// =====================================================================
const DEFAULTS = { quality:'high', shadows:'soft', reflections:true, exposure:1.0, fov:45, autorotate:false,
  theme:'sanctum', fog:0.024, candles:1, shafts:true, dust:true, figures:true, creatures:true, floating:true, mist:true, storm:true, collide:true,
  sfx:0.7, music:0.5, reverb:0.55, bell:true,
  boardStyle:'marble', pieceSet:'ivory', highlights:true, lastmove:true, animSpeed:1 };
const IS_MOBILE = matchMedia('(pointer: coarse)').matches || innerWidth < 700;
if (IS_MOBILE) Object.assign(DEFAULTS, { quality: 'med', reflections: false });
Object.assign(DEFAULTS, { volume: 0.7, floating: false, storm: false, collide: true, quality: IS_MOBILE ? 'med' : 'high', shadows: 'soft', exposure: 1.0, fov: 45, fog: 0.022, candles: 1, shafts: true, dust: true, mist: true, lastmove: true, boardStyle: 'marble', pieceSet: 'ivory', animSpeed: 1 });
const SAVED_KEYS = ['theme', 'highlights', 'autorotate', 'volume', 'figures'];
let S = { ...DEFAULTS };
try { const saved = JSON.parse(localStorage.getItem('sanctum-v3') || '{}'); SAVED_KEYS.forEach(k => { if (k in saved) S[k] = saved[k]; }); } catch (e) {}
Object.keys(DEFAULTS).forEach(k => { if (typeof S[k] !== typeof DEFAULTS[k] || (typeof S[k] === 'number' && !isFinite(S[k]))) S[k] = DEFAULTS[k]; });
function saveSettings() { try { const o = {}; SAVED_KEYS.forEach(k => o[k] = S[k]); localStorage.setItem('sanctum-v3', JSON.stringify(o)); } catch (e) {} }

// =====================================================================
// RENDERER, SCENA, CAMERA
// =====================================================================
const container = document.getElementById('canvas-container');
const scene = new THREE.Scene();
scene.background = new THREE.Color(0x040306);
scene.fog = new THREE.FogExp2(0x050407, S.fog);
const camera = new THREE.PerspectiveCamera(S.fov, (innerWidth || 1) / (innerHeight || 1), 0.3, 160);
const renderer = new THREE.WebGLRenderer({ antialias: true, powerPreference: 'high-performance' });
renderer.setSize(innerWidth || 1, innerHeight || 1);
renderer.outputEncoding = THREE.sRGBEncoding;
renderer.toneMapping = THREE.ACESFilmicToneMapping;
renderer.shadowMap.enabled = true;
renderer.shadowMap.type = THREE.PCFSoftShadowMap;
container.appendChild(renderer.domElement);

const controls = new THREE.OrbitControls(camera, renderer.domElement);
controls.enableDamping = true; controls.dampingFactor = 0.06;
controls.maxPolarAngle = Math.PI / 2 - 0.12; controls.minDistance = 4.5; controls.maxDistance = 30; controls.enablePan = false;
controls.autoRotateSpeed = 0.35;

let isCinemaMode = false;
const cameraPresets = {
  white: { pos: new THREE.Vector3(0, 7.8, 10), target: new THREE.Vector3(0, 0, 0) },
  black: { pos: new THREE.Vector3(0, 7.8, -10), target: new THREE.Vector3(0, 0, 0) },
  top: { pos: new THREE.Vector3(0, 15, 0.01), target: new THREE.Vector3(0, 0, 0) },
  nave: { pos: new THREE.Vector3(0, 5.5, 25), target: new THREE.Vector3(0, 7, -12) },
  apse: { pos: new THREE.Vector3(3.5, 3.2, -9), target: new THREE.Vector3(0, 2.5, -25) },
  cinematic: { pos: new THREE.Vector3(10, 6, 10), target: new THREE.Vector3(0, 0.8, 0) }
};
let currentPreset = 'white';
function uiInsets() { const b = document.getElementById('bottom-bar'); const r = b ? b.getBoundingClientRect() : null;
  return { top: 0, bottom: r && r.height ? Math.max(0, innerHeight - r.top) : 0 }; }
function applyViewOffset() { if (!innerWidth || !innerHeight) return; const ins = uiInsets(); const dy = (ins.top - ins.bottom) / 2; camera.setViewOffset(innerWidth, innerHeight, 0, -dy, innerWidth, innerHeight); camera.updateProjectionMatrix(); }
const BOARD_PRESETS = { white: 1, black: 1, top: 1, cinematic: 1 };
function fittedPreset(key) {
  const p = cameraPresets[key]; if (!BOARD_PRESETS[key] || !innerWidth || !innerHeight) return p;
  const aspect = innerWidth / innerHeight, ins = uiInsets(), free = Math.max(0.35, (innerHeight - ins.top - ins.bottom) / innerHeight);
  const vt = Math.tan(THREE.MathUtils.degToRad(camera.fov) / 2), ht = vt * aspect, vte = vt * free;
  let dir = p.pos.clone().sub(p.target);
  if (aspect < 0.85 && key !== 'top') dir.y *= 2.3; else if (aspect < 1.2 && key !== 'top') dir.y *= 1.4;             // in verticale: inquadratura più dall'alto
  dir.normalize();
  const R = 4.6, elev = Math.abs(dir.y);
  const needH = R / ht, needV = R * (0.55 + 0.45 * elev) / vte;
  const base = p.pos.distanceTo(p.target), dist = Math.max(base * 0.92, needH, needV) * 1.04;
  controls.maxDistance = Math.max(30, dist + 4);
  const pos = p.target.clone().add(dir.multiplyScalar(dist));
  // resta dentro la navata (non attraversare muri e pilastri)
  const hz = Math.hypot(pos.x, pos.z), maxX = 7.0;
  if (Math.abs(pos.x) > maxX) { const zz = Math.sqrt(Math.max(hz * hz - maxX * maxX, 0)); pos.x = Math.sign(pos.x) * maxX; pos.z = Math.sign(pos.z || 1) * zz; }
  pos.z = THREE.MathUtils.clamp(pos.z, -16, 25); pos.y = Math.min(pos.y, 30);
  return { pos, target: p.target.clone() };
}
// Collisione della camera: resta dentro la navata, sopra pavimento/arredi e sotto la volta
const CAM_R = 0.45;
let camCylinders = null;
const buildCamCylinders = () => [
  ...[[-4.95, -4.95], [4.95, -4.95], [-4.95, 4.95], [4.95, 4.95]].map(([x, z]) => ({ x, z, r: 0.75, bottom: -99, top: PL + 3.6 })),
  { x: -7.6, z: -19.6, r: 0.75, bottom: -99, top: SY + 3.6 }, { x: 7.6, z: -19.6, r: 0.75, bottom: -99, top: SY + 3.6 },
  { x: -5.05, z: 0, r: 0.55, bottom: -99, top: PL + 1.9 },
  { x: -6.8, z: 0, r: 0.85, bottom: -99, top: FY + 3.3 }, { x: 6.8, z: 0, r: 0.85, bottom: -99, top: FY + 3.3 },
  { x: 0, z: -16, r: 2.7, bottom: FY + 16.2, top: 99 }, { x: 0, z: 17, r: 2.7, bottom: FY + 16.2, top: 99 } ];
function groundAt(x, z) {
  let g = FY + 2.3;                                                              // sopra le teste dei personaggi
  const ax = Math.abs(x);
  if (ax < 4.8 && Math.abs(z) < 4.8) g = Math.max(g, 1.75);                      // sopra i pezzi
  if (ax > 1.4 && ax < 7.9 && z > 9.6 && z < 23.6) g = Math.max(g, FY + 2.4);      // banchi
  if (z < -17.0) g = Math.max(g, SY + 2.3);                                       // presbiterio
  if (ax < 2.9 && z < -22.6 && z > -25.4) g = Math.max(g, SY + 2.0);              // altare
  if (ax > 5.0 && z > -17.4 && z < -11.6) g = Math.max(g, FY + 3.9);              // stalli del coro
  if (ax > 6.4 && z > -22.6 && z < -20.4) g = Math.max(g, SY + 2.7);              // angeli
  return g;
}
function clampCamera() {
  if (!S.collide) return;
  camCylinders = camCylinders || buildCamCylinders();
  const p = camera.position;
  p.x = THREE.MathUtils.clamp(p.x, -7.25, 7.25);                                 // tra i pilastri della navata
  p.z = THREE.MathUtils.clamp(p.z, -26.2, 26.4);                                 // tra retablo e portale
  if (p.z > 24.4 && p.y > FY + 9.3) p.z = 24.4;                                  // cantoria dell'organo
  const ceil = VY - 1.0 + 14.5 * (1 - Math.pow(p.x / NAVE, 2));                  // volta a sesto acuto
  p.y = Math.min(p.y, ceil, FY + 13 > p.y ? 99 : 99);
  p.y = Math.max(p.y, groundAt(p.x, p.z) + CAM_R);
  camCylinders.forEach(c => { if (p.y < c.bottom || p.y > c.top + CAM_R) return; const dx = p.x - c.x, dz = p.z - c.z, d = Math.hypot(dx, dz), r = c.r + CAM_R;
    if (d < r) { if (c.bottom > -90 && p.y < c.bottom + 0.8) { p.y = Math.min(p.y, c.bottom - CAM_R); return; } const k = d > 1e-4 ? r / d : 1; p.x = c.x + dx * k; p.z = c.z + (d > 1e-4 ? dz * k : r); } });
}
function setCameraPreset(key, instant = false) {
  if (!cameraPresets[key]) return; currentPreset = key; const p = fittedPreset(key);
  if (instant) { camera.position.copy(p.pos); controls.target.copy(p.target); controls.update(); return; }
  const sp = camera.position.clone(), st = controls.target.clone(); let pr = 0;
  (function anim() { pr += 0.022; const t = pr < 0.5 ? 4 * pr ** 3 : 1 - Math.pow(-2 * pr + 2, 3) / 2;
    if (pr <= 1) { camera.position.lerpVectors(sp, p.pos, t); controls.target.lerpVectors(st, p.target, t); requestAnimationFrame(anim); }
    else { camera.position.copy(p.pos); controls.target.copy(p.target); } })();
}
camera.position.set(0, 9, 27); controls.target.set(0, 6, -6); applyViewOffset();

// =====================================================================
// AUDIO: riverbero da cattedrale, organo, coro, campana
// =====================================================================
class GothicAudio {
  constructor() { this.ctx = null; this.soundEnabled = true; this.ambientActive = false; this.timer = null; }
  init() {
    if (this.ctx) return true; const AC = window.AudioContext || window.webkitAudioContext; if (!AC) return false; let c;
    try { c = this.ctx = new AC(); } catch (e) { return false; }
    if (c.state === 'suspended') c.resume();
    this.master = c.createGain(); this.master.connect(c.destination);
    this.reverb = c.createConvolver(); const len = c.sampleRate * 5, buf = c.createBuffer(2, len, c.sampleRate);
    for (let ch = 0; ch < 2; ch++) { const d = buf.getChannelData(ch); for (let i = 0; i < len; i++) d[i] = (Math.random() * 2 - 1) * Math.pow(1 - i / len, 2.6); }
    this.reverb.buffer = buf; this.wet = c.createGain(); this.reverb.connect(this.wet); this.wet.connect(this.master);
    this.sfx = c.createGain(); this.sfx.connect(this.master); this.sfx.connect(this.reverb);
    this.music = c.createGain(); this.music.connect(this.master); this.music.connect(this.reverb);
    this.setVolumes(); return true;
  }
  setVolumes() { if (!this.ctx) return; this.sfx.gain.value = S.volume; this.music.gain.value = S.volume * 0.6; this.wet.gain.value = 0.75; }
  tone(type, f1, f2, vol, dur, delay = 0) {
    const t0 = this.ctx.currentTime + delay, o = this.ctx.createOscillator(), g = this.ctx.createGain();
    o.type = type; o.frequency.setValueAtTime(f1, t0); if (f2) o.frequency.exponentialRampToValueAtTime(f2, t0 + dur);
    g.gain.setValueAtTime(vol, t0); g.gain.exponentialRampToValueAtTime(0.0008, t0 + dur);
    o.connect(g); g.connect(this.sfx); o.start(t0); o.stop(t0 + dur + 0.05);
  }
  playMove() { if (!this.soundEnabled || !this.init()) return; this.tone('triangle', 150, 60, 0.35, 0.2); this.tone('sine', 90, 40, 0.3, 0.25); }
  playCapture() { if (!this.soundEnabled || !this.init()) return; this.tone('sawtooth', 110, 30, 0.3, 0.35); this.tone('square', 70, 25, 0.15, 0.4); }
  playCheck() { if (!this.soundEnabled || !this.init()) return; [392, 466.16, 587.33].forEach((f, i) => this.tone('sine', f, null, 0.16, 1.2, i * 0.09)); }
  bell() {
    if (!this.soundEnabled || !S.bell || !this.init()) return;
    [0.5, 1, 1.19, 1.5, 2, 2.52, 3.01].forEach((m, i) => this.tone('sine', 196 * m, null, 0.22 / (i + 1), 6 - i * 0.5));
    setTimeout(() => [0.5, 1, 1.19, 1.5, 2].forEach((m, i) => this.tone('sine', 196 * m, null, 0.18 / (i + 1), 5)), 2200);
  }
  crack() {
    if (!this.soundEnabled || !this.init()) return; const c = this.ctx, t = c.currentTime, len = c.sampleRate * 0.5, buf = c.createBuffer(1, len, c.sampleRate), d = buf.getChannelData(0);
    for (let i = 0; i < len; i++) d[i] = (Math.random() * 2 - 1) * Math.pow(1 - i / len, 6);
    const src = c.createBufferSource(); src.buffer = buf; const hp = c.createBiquadFilter(); hp.type = 'highpass'; hp.frequency.value = 900; const g = c.createGain(); g.gain.value = 0.55;
    src.connect(hp); hp.connect(g); g.connect(this.sfx); src.start(t);
    this.tone('sine', 120, 40, 0.5, 0.35); this.tone('triangle', 70, 30, 0.35, 0.5);
  }
  zap(color) {
    if (!this.soundEnabled || !this.init()) return; const b = color === 'w' ? 660 : 330;
    this.tone('sine', b, b * 2.2, 0.12, 0.35); this.tone('triangle', b * 1.5, b * 0.7, 0.06, 0.4, 0.05);
  }
  thunder(power = 1) {
    if (!this.soundEnabled || !this.init()) return; const c = this.ctx, t = c.currentTime, len = c.sampleRate * 4, buf = c.createBuffer(1, len, c.sampleRate), d = buf.getChannelData(0);
    let last = 0; for (let i = 0; i < len; i++) { last = last * 0.985 + (Math.random() * 2 - 1) * 0.15; d[i] = last; }
    const src = c.createBufferSource(); src.buffer = buf; const lp = c.createBiquadFilter(); lp.type = 'lowpass'; lp.frequency.setValueAtTime(420, t); lp.frequency.exponentialRampToValueAtTime(90, t + 3.5);
    const g = c.createGain(); g.gain.setValueAtTime(0.0001, t); g.gain.exponentialRampToValueAtTime(0.9 * power, t + 0.12); g.gain.exponentialRampToValueAtTime(0.25 * power, t + 1.2); g.gain.exponentialRampToValueAtTime(0.0001, t + 3.9);
    src.connect(lp); lp.connect(g); g.connect(this.sfx); src.start(t); src.stop(t + 4);
  }
  voice(freq, dur, choir) {
    const c = this.ctx, t = c.currentTime, g = c.createGain();
    g.gain.setValueAtTime(0.0001, t); g.gain.linearRampToValueAtTime(choir ? 0.05 : 0.035, t + 1.8); g.gain.linearRampToValueAtTime(0.0001, t + dur);
    g.connect(this.music);
    if (choir) {
      const o = c.createOscillator(); o.type = 'sawtooth'; o.frequency.value = freq;
      const lfo = c.createOscillator(), lg = c.createGain(); lfo.frequency.value = 5; lg.gain.value = freq * 0.008; lfo.connect(lg); lg.connect(o.frequency);
      [[700, 7], [1150, 9], [2800, 12]].forEach(([f, q]) => { const b = c.createBiquadFilter(); b.type = 'bandpass'; b.frequency.value = f; b.Q.value = q; o.connect(b); b.connect(g); });
      o.start(t); lfo.start(t); o.stop(t + dur); lfo.stop(t + dur);
    } else {
      const lp = c.createBiquadFilter(); lp.type = 'lowpass'; lp.frequency.value = 1400; lp.connect(g);
      [[1, 'sawtooth'], [2, 'square'], [0.5, 'sine']].forEach(([m, type], i) => { const o = c.createOscillator(); o.type = type; o.frequency.value = freq * m; o.detune.value = (i - 1) * 4; o.connect(lp); o.start(t); o.stop(t + dur); });
    }
  }
  toggleAmbient() {
    if (!this.init()) return false; this.ambientActive = !this.ambientActive;
    if (this.ambientActive) {
      const chords = [[110, 164.81, 220, 261.63], [98, 146.83, 196, 246.94], [87.31, 130.81, 174.61, 220], [103.83, 155.56, 207.65, 261.63], [82.41, 123.47, 164.81, 207.65]];
      let ci = 0;
      const play = () => { if (!this.ambientActive) return; const ch = chords[ci++ % chords.length];
        ch.forEach((f, i) => this.voice(f, 7, false)); this.voice(ch[2] * 2, 6.5, true); this.voice(ch[3] * 2, 6.5, true); };
      play(); this.timer = setInterval(play, 6500);
    } else clearInterval(this.timer);
    return this.ambientActive;
  }
}
const audio = new GothicAudio();

// =====================================================================
// LOGICA SCACCHI
// =====================================================================
const game = new Chess();
let gameMode = 'pvp', aiDifficulty = 'med', selectedSquare = null, legalMoves = [], isAiThinking = false, lastMove = null;
let timerMode = 'off', timeWhite = 180, timeBlack = 180, timerInterval = null;
function startTimerInterval() {
  clearInterval(timerInterval); if (timerMode === 'off') return;
  timerInterval = setInterval(() => {
    if (game.game_over()) return clearInterval(timerInterval);
    if (game.turn() === 'w') { if (--timeWhite <= 0) { timeWhite = 0; clearInterval(timerInterval); triggerGameOver('Tempo scaduto!', 'Il tempo del Bianco è finito. Vince il Nero!'); } }
    else { if (--timeBlack <= 0) { timeBlack = 0; clearInterval(timerInterval); triggerGameOver('Tempo scaduto!', 'Il tempo del Nero è finito. Vince il Bianco!'); } }
    updateTimerUI();
  }, 1000);
}
function updateTimerUI() { const f = s => `${String(Math.floor(s / 60)).padStart(2, '0')}:${String(s % 60).padStart(2, '0')}`; const w = document.getElementById('timer-white'), b = document.getElementById('timer-black'); w.innerText = f(timeWhite); b.innerText = f(timeBlack); w.classList.toggle('run', game.turn() === 'w'); b.classList.toggle('run', game.turn() === 'b'); }
const pieceValues = { p: 10, n: 30, b: 30, r: 50, q: 90, k: 900 };
function evaluateBoard(board) { let t = 0; for (const row of board) for (const p of row) if (p) t += p.color === 'w' ? pieceValues[p.type] : -pieceValues[p.type]; return t; }
function minimax(depth, g, isMax, a, b) {
  if (depth === 0 || g.game_over()) return evaluateBoard(g.board());
  const mv = g.moves();
  if (isMax) { let m = -99999; for (const x of mv) { g.move(x); const e = minimax(depth - 1, g, false, a, b); g.undo(); m = Math.max(m, e); a = Math.max(a, e); if (b <= a) break; } return m; }
  let m = 99999; for (const x of mv) { g.move(x); const e = minimax(depth - 1, g, true, a, b); g.undo(); m = Math.min(m, e); b = Math.min(b, e); if (b <= a) break; } return m;
}
function getBestMoveAI() {
  const moves = game.moves({ verbose: true }); if (!moves.length) return null;
  if (aiDifficulty === 'easy') return moves[Math.floor(Math.random() * moves.length)];
  const depth = aiDifficulty === 'med' ? 2 : 3, max = game.turn() === 'w'; let best = null, bv = max ? -99999 : 99999;
  moves.sort(() => Math.random() - 0.5);
  for (const m of moves) { game.move(m); const v = minimax(depth - 1, game, !max, -1e5, 1e5); game.undo(); if (max ? v > bv : v < bv) { bv = v; best = m; } }
  return best || moves[0];
}

// =====================================================================
// TEXTURE PROCEDURALI (albedo + normal map)
// =====================================================================
const V2 = THREE.Vector2;
function mkCanvas(w, h) { const c = document.createElement('canvas'); c.width = w; c.height = h || w; return c; }
function heightToNormal(hc, strength) {
  const S2 = hc.width, hd = hc.getContext('2d').getImageData(0, 0, S2, S2).data;
  const nc = mkCanvas(S2), ng = nc.getContext('2d'), out = ng.createImageData(S2, S2), o = out.data;
  const H = (x, y) => hd[((((y % S2) + S2) % S2) * S2 + (((x % S2) + S2) % S2)) * 4] / 255;
  for (let y = 0; y < S2; y++) for (let x = 0; x < S2; x++) {
    const dx = (H(x - 1, y) - H(x + 1, y)) * strength, dy = (H(x, y + 1) - H(x, y - 1)) * strength, l = Math.hypot(dx, dy, 1), i = (y * S2 + x) * 4;
    o[i] = (dx / l * 0.5 + 0.5) * 255; o[i + 1] = (dy / l * 0.5 + 0.5) * 255; o[i + 2] = (1 / l * 0.5 + 0.5) * 255; o[i + 3] = 255;
  }
  ng.putImageData(out, 0, 0); return nc;
}
function texSet(size, paint, strength, rep) {
  const c = mkCanvas(size), h = mkCanvas(size); paint(c.getContext('2d'), h.getContext('2d'), size);
  const map = new THREE.CanvasTexture(c); map.encoding = THREE.sRGBEncoding;
  const normalMap = new THREE.CanvasTexture(heightToNormal(h, strength));
  [map, normalMap].forEach(t => { t.wrapS = t.wrapT = THREE.RepeatWrapping; t.repeat.set(rep, rep); t.anisotropy = 8; });
  return { map, normalMap };
}
function speckle(g, hg, x, y, w, h, n, dark) {
  for (let i = 0; i < n; i++) { const px = x + Math.random() * w, py = y + Math.random() * h, s = 1 + Math.random() * 3, v = Math.random();
    g.fillStyle = `rgba(${dark ? 0 : 255},${dark ? 0 : 250},${dark ? 0 : 240},${v * 0.08})`; g.fillRect(px, py, s, s);
    hg.fillStyle = `rgba(${v > 0.5 ? 255 : 0},${v > 0.5 ? 255 : 0},${v > 0.5 ? 255 : 0},0.12)`; hg.fillRect(px, py, s, s); }
}
// Pietra squadrata (pareti) – un riquadro texture = 4 unità di mondo
const ashlar = texSet(IS_MOBILE ? 512 : 1024, (g, hg, s) => {
  g.fillStyle = '#141216'; g.fillRect(0, 0, s, s); hg.fillStyle = '#1a1a1a'; hg.fillRect(0, 0, s, s);
  const rows = 8, cols = 4, bh = s / rows, bw = s / cols;
  for (let r = 0; r < rows; r++) for (let c = -1; c <= cols; c++) {
    const x = c * bw + (r % 2) * bw / 2, y = r * bh, l = 26 + Math.random() * 10, hue = 30 + Math.random() * 20;
    g.fillStyle = `hsl(${hue},7%,${l}%)`; g.fillRect(x + 3, y + 3, bw - 6, bh - 6);
    const hv = 150 + Math.random() * 60; hg.fillStyle = `rgb(${hv},${hv},${hv})`; hg.fillRect(x + 3, y + 3, bw - 6, bh - 6);
    hg.fillStyle = 'rgba(0,0,0,0.25)'; hg.fillRect(x + 3, y + bh - 8, bw - 6, 5);
    speckle(g, hg, x + 3, y + 3, bw - 6, bh - 6, 70, Math.random() > 0.3);
  }
  for (let i = 0; i < 40; i++) { const x = Math.random() * s; const gr = g.createLinearGradient(x, 0, x, s); gr.addColorStop(0, 'rgba(0,0,0,0)'); gr.addColorStop(1, 'rgba(0,0,0,0.22)'); g.fillStyle = gr; g.fillRect(x, Math.random() * s, 2 + Math.random() * 5, s); }
}, 3.5, 0.25);
// Pietra liscia (colonne, archi, statue)
const dressed = texSet(256, (g, hg, s) => {
  g.fillStyle = '#4a4540'; g.fillRect(0, 0, s, s); hg.fillStyle = '#808080'; hg.fillRect(0, 0, s, s);
  speckle(g, hg, 0, 0, s, s, 1600, true); speckle(g, hg, 0, 0, s, s, 600, false);
  for (let i = 0; i < 6; i++) { g.strokeStyle = 'rgba(0,0,0,0.12)'; g.lineWidth = 1; g.beginPath(); let x = Math.random() * s, y = 0; g.moveTo(x, y); while (y < s) { x += (Math.random() - 0.5) * 10; y += 8; g.lineTo(x, y); } g.stroke(); }
}, 2, 0.5);
// Pavimento: lastre irregolari di pietra (niente scacchi, per non confondersi con la scacchiera)
const floorSet = texSet(512, (g, hg, s) => {
  g.fillStyle = '#0e0c0e'; g.fillRect(0, 0, s, s); hg.fillStyle = '#000'; hg.fillRect(0, 0, s, s);
  const rows = 5, rh = s / rows;
  for (let r = 0; r < rows; r++) { let x = -Math.random() * 60;
    while (x < s) { const w = 70 + Math.random() * 90, l = 15 + Math.random() * 6, hue = 28 + Math.random() * 14;
      [0, s].forEach(off => { g.fillStyle = `hsl(${hue},8%,${l}%)`; g.fillRect(x + 2 - off, r * rh + 2, w - 4, rh - 4);
        const hv = 160 + Math.random() * 50; hg.fillStyle = `rgb(${hv},${hv},${hv})`; hg.fillRect(x + 2 - off, r * rh + 2, w - 4, rh - 4); });
      speckle(g, hg, x + 2, r * rh + 2, w - 4, rh - 4, 90, true);
      for (let v = 0; v < 2; v++) { g.strokeStyle = 'rgba(220,210,190,0.05)'; g.lineWidth = 0.8; g.beginPath(); g.moveTo(x + Math.random() * w, r * rh); g.bezierCurveTo(x + Math.random() * w, r * rh + rh * 0.3, x + Math.random() * w, r * rh + rh * 0.7, x + Math.random() * w, r * rh + rh); g.stroke(); }
      if (Math.random() > 0.75) { hg.strokeStyle = 'rgba(0,0,0,0.7)'; g.strokeStyle = 'rgba(0,0,0,0.5)'; [g, hg].forEach(c => { c.beginPath(); c.moveTo(x + Math.random() * w, r * rh + Math.random() * rh); c.lineTo(x + Math.random() * w, r * rh + Math.random() * rh); c.stroke(); }); }
      x += w; } }
  const gr = g.createRadialGradient(s / 2, s / 2, 0, s / 2, s / 2, s * 0.7); gr.addColorStop(0, 'rgba(0,0,0,0)'); gr.addColorStop(1, 'rgba(0,0,0,0.15)'); g.fillStyle = gr; g.fillRect(0, 0, s, s);
}, 4, 0.2);
// Legno di quercia scura
const woodSet = texSet(256, (g, hg, s) => {
  g.fillStyle = '#2a1a10'; g.fillRect(0, 0, s, s); hg.fillStyle = '#808080'; hg.fillRect(0, 0, s, s);
  for (let i = 0; i < 90; i++) { const y = Math.random() * s, a = Math.random() * 0.25; g.strokeStyle = `rgba(${Math.random() > 0.5 ? '10,5,2' : '70,45,25'},${a})`; hg.strokeStyle = `rgba(0,0,0,${a})`; g.lineWidth = hg.lineWidth = 1 + Math.random() * 2;
    [g, hg].forEach(c => { c.beginPath(); c.moveTo(0, y); for (let x = 0; x <= s; x += 16) c.lineTo(x, y + Math.sin(x * 0.03 + i) * 3); c.stroke(); }); }
}, 1.5, 1);
// Tessuto (saio)
const clothSet = texSet(128, (g, hg, s) => {
  g.fillStyle = '#ffffff'; g.fillRect(0, 0, s, s); hg.fillStyle = '#808080'; hg.fillRect(0, 0, s, s);
  for (let i = 0; i < s; i += 2) { hg.fillStyle = `rgba(255,255,255,${0.1 + Math.random() * 0.15})`; hg.fillRect(0, i, s, 1); hg.fillStyle = `rgba(0,0,0,${0.1 + Math.random() * 0.1})`; hg.fillRect(i, 0, 1, s); }
  for (let i = 0; i < 400; i++) { g.fillStyle = `rgba(0,0,0,${Math.random() * 0.12})`; g.fillRect(Math.random() * s, Math.random() * s, 2, 2); }
}, 1.2, 3);
// Marmo per la scacchiera
function marbleTex(base, vein) {
  const c = mkCanvas(256), g = c.getContext('2d'); g.fillStyle = base; g.fillRect(0, 0, 256, 256);
  for (let i = 0; i < 14; i++) { g.strokeStyle = vein; g.globalAlpha = 0.08 + Math.random() * 0.25; g.lineWidth = 0.5 + Math.random() * 2; g.beginPath(); let x = Math.random() * 256, y = 0; g.moveTo(x, y); while (y < 256) { x += (Math.random() - 0.5) * 30; y += 10 + Math.random() * 10; g.lineTo(x, y); } g.stroke(); }
  g.globalAlpha = 1; const t = new THREE.CanvasTexture(c); t.encoding = THREE.sRGBEncoding; return t;
}
const marbleW = marbleTex('#efe8d8', '#8a7c6a'), marbleB = marbleTex('#2a2420', '#8a7556');
// Vetrate
function lancetTex(seed) {
  const pal = [['#7a0c1c', '#10307a', '#a8700f', '#0c5230', '#3e1460'], ['#123070', '#8a1020', '#b0841a', '#4a1666', '#0e4a3a'], ['#5a0e60', '#a01818', '#1a3a8a', '#9a7a14', '#105030']][seed % 3];
  const c = mkCanvas(128, 512), g = c.getContext('2d'), w = 128, h = 512;
  for (let y = 0; y < h; y += 14) for (let x = 0; x < w; x += 14) { g.fillStyle = pal[(Math.random() * 5) | 0]; g.globalAlpha = 0.6 + Math.random() * 0.4; g.fillRect(x, y, 14, 14); }
  g.globalAlpha = 1;
  for (let m = 0; m < 3; m++) { const cy = 110 + m * 150; g.fillStyle = pal[(m + seed) % 5]; g.beginPath(); g.arc(w / 2, cy, 40, 0, 7); g.fill();
    g.fillStyle = '#c9a050'; g.beginPath(); g.arc(w / 2, cy - 16, 13, 0, 7); g.fill(); g.fillStyle = '#d8b890'; g.beginPath(); g.arc(w / 2, cy - 16, 7, 0, 7); g.fill();
    g.fillStyle = pal[(m + seed + 2) % 5]; g.beginPath(); g.moveTo(w / 2 - 20, cy + 36); g.lineTo(w / 2, cy - 8); g.lineTo(w / 2 + 20, cy + 36); g.fill();
    g.strokeStyle = '#0b0806'; g.lineWidth = 5; g.beginPath(); g.arc(w / 2, cy, 40, 0, 7); g.stroke(); }
  g.strokeStyle = '#0b0806'; g.lineWidth = 2; for (let y = 0; y < h; y += 14) { g.beginPath(); g.moveTo(0, y + Math.random() * 3); g.lineTo(w, y + Math.random() * 3); g.stroke(); }
  for (let x = 0; x < w; x += 14) { g.beginPath(); g.moveTo(x, 0); g.lineTo(x, h); g.stroke(); }
  g.lineWidth = 10; g.strokeRect(0, 0, w, h); const t = new THREE.CanvasTexture(c); t.encoding = THREE.sRGBEncoding; return t;
}
const roseTex = (() => {
  const c = mkCanvas(512), g = c.getContext('2d'), cx = 256, cols = ['#10307a', '#7a0c1c', '#a8700f', '#3e1460', '#0c5230', '#8a1040'];
  g.fillStyle = '#000'; g.fillRect(0, 0, 512, 512); g.save(); g.beginPath(); g.arc(cx, cx, 254, 0, 7); g.clip();
  for (let r = 6; r > 0; r--) { const R = r * 42, n = 12 + r * 2; for (let i = 0; i < n; i++) { g.fillStyle = cols[(i + r) % 6]; g.globalAlpha = 0.7 + Math.random() * 0.3; g.beginPath(); g.moveTo(cx, cx); g.arc(cx, cx, R, i / n * 6.283, (i + 1) / n * 6.283); g.closePath(); g.fill(); g.globalAlpha = 1; g.strokeStyle = '#0b0806'; g.lineWidth = 3; g.stroke(); } }
  g.fillStyle = '#c9a050'; g.beginPath(); g.arc(cx, cx, 34, 0, 7); g.fill(); g.restore();
  const t = new THREE.CanvasTexture(c); t.encoding = THREE.sRGBEncoding; return t;
})();
const glowTex = (() => { const c = mkCanvas(64), g = c.getContext('2d'), gr = g.createRadialGradient(32, 32, 0, 32, 32, 32);
  gr.addColorStop(0, 'rgba(255,255,255,1)'); gr.addColorStop(0.2, 'rgba(255,255,255,0.45)'); gr.addColorStop(1, 'rgba(255,255,255,0)'); g.fillStyle = gr; g.fillRect(0, 0, 64, 64); return new THREE.CanvasTexture(c); })();
const smokeTex = (() => { const c = mkCanvas(64), g = c.getContext('2d'); for (let i = 0; i < 10; i++) { const x = 20 + Math.random() * 24, y = 20 + Math.random() * 24, gr = g.createRadialGradient(x, y, 0, x, y, 20); gr.addColorStop(0, 'rgba(255,255,255,0.25)'); gr.addColorStop(1, 'rgba(255,255,255,0)'); g.fillStyle = gr; g.fillRect(0, 0, 64, 64); } return new THREE.CanvasTexture(c); })();
function makeGlow(color, size, opacity = 0.9) {
  const s = new THREE.Sprite(new THREE.SpriteMaterial({ map: glowTex, color, transparent: true, opacity, blending: THREE.AdditiveBlending, depthWrite: false, fog: true })); s.renderOrder = 6;
  s.scale.set(size, size, 1); return s;
}

// =====================================================================
// MATERIALI
// =====================================================================
const wallMat = new THREE.MeshStandardMaterial({ map: ashlar.map, normalMap: ashlar.normalMap, color: 0xa29a92, roughness: 0.92, side: THREE.DoubleSide });
const stoneMat = new THREE.MeshStandardMaterial({ map: dressed.map, normalMap: dressed.normalMap, color: 0xaaa298, roughness: 0.82 });
const statueMat = new THREE.MeshStandardMaterial({ map: dressed.map, normalMap: dressed.normalMap, color: 0xc4bcb0, roughness: 0.78 });
const darkStoneMat = new THREE.MeshStandardMaterial({ map: dressed.map, normalMap: dressed.normalMap, color: 0x58524e, roughness: 0.6, metalness: 0.1 });
const floorMat = new THREE.MeshStandardMaterial({ map: floorSet.map, normalMap: floorSet.normalMap, roughness: 0.55, metalness: 0.05 });
const woodMat = new THREE.MeshStandardMaterial({ map: woodSet.map, normalMap: woodSet.normalMap, color: 0xb09080, roughness: 0.62 });
const ironMat = new THREE.MeshStandardMaterial({ color: 0x2a282c, roughness: 0.55, metalness: 0.5 });
const goldMat = new THREE.MeshStandardMaterial({ color: 0xc8a060, roughness: 0.38, metalness: 0.55, emissive: 0x1a1006 });
const bronzeMat = new THREE.MeshStandardMaterial({ color: 0x7a5630, roughness: 0.45, metalness: 0.5 });
const waxMat = new THREE.MeshStandardMaterial({ color: 0xe8dcc0, roughness: 0.55, emissive: 0x2a1808 });
const velvetMat = new THREE.MeshStandardMaterial({ color: 0x3a060c, roughness: 0.95, normalMap: clothSet.normalMap });
const ceilingMat = new THREE.MeshStandardMaterial({ map: ashlar.map, normalMap: ashlar.normalMap, color: 0x5a5458, roughness: 0.95, side: THREE.DoubleSide });
const flameMat = new THREE.MeshBasicMaterial({ color: new THREE.Color(2.0, 1.05, 0.38), fog: true });
const flameCoreMat = new THREE.MeshBasicMaterial({ color: new THREE.Color(3.2, 2.7, 2.0) });
const cloth = (color, extra = {}) => new THREE.MeshStandardMaterial({ color, roughness: 0.96, normalMap: clothSet.normalMap, normalScale: new V2(0.6, 0.6), side: THREE.DoubleSide, ...extra });
const habitBlack = cloth(0x141215), habitBrown = cloth(0x2a1d14), surpliceMat = cloth(0x9c968a), albMat = cloth(0xb8b0a0), chasubleMat = cloth(0x4a0a12, { roughness: 0.85 });
const ropeMat = new THREE.MeshStandardMaterial({ color: 0x6a5a40, roughness: 1 });
const darkFaceMat = new THREE.MeshBasicMaterial({ color: 0x000000 });
const skinMat = new THREE.MeshStandardMaterial({ color: 0x8a7060, roughness: 0.7 });
const glassMats = [];

// =====================================================================
// GEOMETRIE GOTICHE
// =====================================================================
function uvScale(geo, su, sv) { const uv = geo.attributes.uv; if (!uv) return geo; for (let i = 0; i < uv.count; i++) uv.setXY(i, uv.getX(i) * su, uv.getY(i) * sv); return geo; }
function fitUV(geo) { geo.computeBoundingBox(); const b = geo.boundingBox, w = b.max.x - b.min.x, h = b.max.y - b.min.y, p = geo.attributes.position, uv = geo.attributes.uv;
  for (let i = 0; i < p.count; i++) uv.setXY(i, (p.getX(i) - b.min.x) / w, (p.getY(i) - b.min.y) / h); return geo; }
function dedupe(pts) { return pts.filter((p, i) => i === 0 || p.distanceTo(pts[i - 1]) > 1e-5); }
// arco acuto: centri in (±a, yc), raggio R
function arcCurve(a, R, yc, n = 14) {
  const pts = [], p0 = Math.acos(-a / R), q0 = Math.PI - p0;
  for (let i = 0; i <= n; i++) { const p = Math.PI + (p0 - Math.PI) * i / n; pts.push(new V2(a + R * Math.cos(p), yc + R * Math.sin(p))); }
  for (let i = 1; i <= n; i++) { const p = q0 * (1 - i / n); pts.push(new V2(-a + R * Math.cos(p), yc + R * Math.sin(p))); }
  return pts;
}
function archFill(a, R, y0, legH) { return dedupe([new V2(a - R, y0), ...arcCurve(a, R, y0 + legH), new V2(R - a, y0)]); }
function archApex(a, R) { return R * Math.sin(Math.acos(-a / R)); }
function archRing(a, Rin, Rout, legH, depth) {
  const outer = [new V2(a - Rout, 0), ...arcCurve(a, Rout, legH), new V2(Rout - a, 0)];
  const inner = [new V2(Rin - a, 0), ...arcCurve(a, Rin, legH).reverse(), new V2(a - Rin, 0)];
  const g = new THREE.ExtrudeGeometry(new THREE.Shape(dedupe([...outer, ...inner])), { depth, bevelEnabled: false, curveSegments: 1 });
  g.translate(0, 0, -depth / 2); uvScale(g, 0.25, 0.25); return g;
}
function extrude(shape, depth, uvs = 0.25) { const g = new THREE.ExtrudeGeometry(shape, { depth, bevelEnabled: false, curveSegments: 6 }); g.translate(0, 0, -depth / 2); return uvScale(g, uvs, uvs); }
function box(w, h, d, mat, x, y, z, parent, cast = true) { const m = new THREE.Mesh(uvScale(new THREE.BoxGeometry(w, h, d), Math.max(w, d) / 4, h / 4), mat); m.position.set(x, y, z); m.castShadow = cast; m.receiveShadow = true; parent.add(m); return m; }
function cyl(rt, rb, h, seg, mat, x, y, z, parent, cast = true) { const m = new THREE.Mesh(uvScale(new THREE.CylinderGeometry(rt, rb, h, seg), Math.PI * 2 * rb / 4, h / 4), mat); m.position.set(x, y, z); m.castShadow = cast; m.receiveShadow = true; parent.add(m); return m; }
function lathe(prof, seg, mat) { const g = new THREE.LatheGeometry(prof.map(p => new V2(p[0], p[1])), seg); uvScale(g, 2, 1); const m = new THREE.Mesh(g, mat); m.castShadow = true; m.receiveShadow = true; return m; }

// Unisce le geometrie statiche per materiale (molte meno draw call)
function bake(group) {
  group.updateMatrixWorld(true); const buckets = new Map(), victims = [];
  group.traverse(o => { if (o.isMesh && !o.userData.noBake && !o.material.transparent) {
    const key = o.material.uuid + '|' + (o.castShadow ? 1 : 0);
    if (!buckets.has(key)) buckets.set(key, { mat: o.material, cast: o.castShadow, geos: [] });
    const g = o.geometry.index ? o.geometry.toNonIndexed() : o.geometry.clone(); g.applyMatrix4(o.matrixWorld);
    if (o.matrixWorld.determinant() < 0) { const p = g.attributes.position.array, n = g.attributes.normal.array; for (let i = 0; i < p.length; i += 9) for (let k = 0; k < 3; k++) { [p[i + 3 + k], p[i + 6 + k]] = [p[i + 6 + k], p[i + 3 + k]]; [n[i + 3 + k], n[i + 6 + k]] = [n[i + 6 + k], n[i + 3 + k]]; } }
    buckets.get(key).geos.push(g); victims.push(o); } });
  victims.forEach(o => o.parent.remove(o));
  buckets.forEach(b => {
    let n = 0; b.geos.forEach(g => n += g.attributes.position.count);
    const P = new Float32Array(n * 3), N = new Float32Array(n * 3), U = new Float32Array(n * 2); let off = 0;
    b.geos.forEach(g => { P.set(g.attributes.position.array, off * 3); if (g.attributes.normal) N.set(g.attributes.normal.array, off * 3); if (g.attributes.uv) U.set(g.attributes.uv.array, off * 2); off += g.attributes.position.count; g.dispose(); });
    const geo = new THREE.BufferGeometry(); geo.setAttribute('position', new THREE.BufferAttribute(P, 3)); geo.setAttribute('normal', new THREE.BufferAttribute(N, 3)); geo.setAttribute('uv', new THREE.BufferAttribute(U, 2));
    const m = new THREE.Mesh(geo, b.mat); m.castShadow = b.cast; m.receiveShadow = true; group.add(m);
  });
}

// =====================================================================
// CANDELE
// =====================================================================
const flames = [], flickerLights = [];
function addCandle(parent, x, y, z, h = 0.4, s = 1) {
  const r = 0.045 * s;
  const wax = cyl(r, r * 1.05, h, 10, waxMat, x, y + h / 2, z, parent, false);
  for (let i = 0; i < 3; i++) { const d = new THREE.Mesh(new THREE.SphereGeometry(r * 0.35, 6, 6), waxMat); const a = Math.random() * 6.28; d.position.set(x + Math.cos(a) * r, y + h - Math.random() * h * 0.5, z + Math.sin(a) * r); d.scale.y = 2.2; parent.add(d); }
  const fg = new THREE.Group(); fg.position.set(x, y + h + 0.07 * s, z); parent.add(fg);
  const flame = new THREE.Mesh(new THREE.SphereGeometry(1, 10, 8), flameMat); flame.scale.set(0.028 * s, 0.075 * s, 0.028 * s); flame.userData.noBake = true; fg.add(flame);
  const core = new THREE.Mesh(new THREE.SphereGeometry(1, 8, 6), flameCoreMat); core.scale.set(0.012 * s, 0.035 * s, 0.012 * s); core.position.y = -0.02 * s; core.userData.noBake = true; fg.add(core);
  const glow = makeGlow(0xff9a3c, 0.7 * s, 0.75); fg.add(glow);
  flames.push({ fg, glow, base: 0.7 * s, phase: Math.random() * 10 });
}

// =====================================================================
// ARCHITETTURA (y pavimento = -2)
// =====================================================================
const staticG = new THREE.Group(); scene.add(staticG);
const FY = -2, NAVE = 9, AISLE = 17, LEN = 28, BAY = 8;
const piersZ = [-20, -12, -4, 4, 12, 20], bayZ = [-24, -16, -8, 0, 8, 16, 24];

// Pavimento
const floorGeo = new THREE.PlaneGeometry(AISLE * 2, LEN * 2); uvScale(floorGeo, AISLE * 2, LEN * 2);
const floor = new THREE.Mesh(floorGeo, floorMat); floor.rotation.x = -Math.PI / 2; floor.position.y = FY; floor.receiveShadow = true; staticG.add(floor);
// Lastre tombali nel pavimento
for (let i = 0; i < 6; i++) { const lz = -6 + i * 3.5 * (i % 2 ? 1 : -1); box(1.2, 0.04, 2.2, darkStoneMat, (i % 2 ? 1 : -1) * 12.8, FY + 0.01, lz, staticG, false); }

// Pareti della navata con arcate e finestre del cleristorio
function bayWall(side, zc, idx) {
  const g = new THREE.Group(); g.position.set(side * NAVE, FY, zc); g.rotation.y = -Math.PI / 2; staticG.add(g);
  const sh = new THREE.Shape(dedupe([new V2(-4, 0), new V2(-3, 0), ...arcCurve(3, 6, 7), new V2(3, 0), new V2(4, 0), new V2(4, 24), new V2(-4, 24)]));
  sh.holes.push(new THREE.Path(archFill(1.5, 3, 14, 3.2).reverse()));
  const wall = new THREE.Mesh(extrude(sh, 1.0), wallMat); wall.receiveShadow = true; g.add(wall);
  // modanature dell'arco
  [[6, 6.35, 1.4], [6.35, 6.7, 1.15]].forEach(([ri, ro, d]) => { const m = new THREE.Mesh(archRing(3, ri, ro, 7, d), stoneMat); m.castShadow = true; g.add(m); });
  const m2 = new THREE.Mesh(archRing(1.5, 3, 3.35, 3.2, 1.25), stoneMat); m2.position.y = 14; g.add(m2);
  // vetrata
  const gm = new THREE.MeshBasicMaterial({ map: lancetTex(idx), side: THREE.DoubleSide, fog: true }); glassMats.push(gm);
  const glass = new THREE.Mesh(fitUV(new THREE.ShapeGeometry(new THREE.Shape(archFill(1.5, 3, 14, 3.2)))), gm); g.add(glass);
  const mull = box(0.12, 5.6, 0.25, stoneMat, 0, 16.8, 0, g, false);
  const tr = new THREE.Mesh(new THREE.TorusGeometry(0.55, 0.07, 6, 20), stoneMat); tr.position.set(0, 19.3, 0); g.add(tr);
  // cornice/marcapiano
  box(8, 0.35, 1.5, stoneMat, 0, 13.2, 0, g, false);
  // finto triforio: piccole arcate cieche
  for (let k = -1; k <= 1; k++) { const t = new THREE.Mesh(archRing(0.4, 0.8, 0.95, 0.9, 0.3), stoneMat); t.position.set(k * 2.2, 10.7, side > 0 ? 0.55 : -0.55); g.add(t); }
  return g;
}
let wi = 0; [-1, 1].forEach(side => bayZ.forEach(z => bayWall(side, z, wi++)));

// Pilastri polistili
function pier(x, z) {
  const g = new THREE.Group(); g.position.set(x, FY, z); staticG.add(g);
  const base = cyl(1.25, 1.35, 0.9, 8, stoneMat, 0, 0.45, 0, g); base.rotation.y = Math.PI / 8;
  cyl(1.1, 1.25, 0.3, 16, stoneMat, 0, 1.05, 0, g);
  cyl(0.82, 0.82, 23.5, 16, stoneMat, 0, 12.9, 0, g);
  for (let i = 0; i < 8; i++) { const a = i / 8 * Math.PI * 2, r = i % 2 ? 0.78 : 0.88, rr = i % 2 ? 0.17 : 0.27; cyl(rr, rr, 23.5, 10, stoneMat, Math.cos(a) * r, 12.9, Math.sin(a) * r, g); }
  [7, 24].forEach(y => { const cap = lathe([[0.001, 0], [0.95, 0], [1.15, 0.25], [1.35, 0.55], [1.4, 0.7], [0.001, 0.7]], 16, stoneMat); cap.position.y = y - 0.7; g.add(cap);
    const leaves = new THREE.Mesh(new THREE.TorusGeometry(1.0, 0.13, 6, 16), stoneMat); leaves.rotation.x = Math.PI / 2; leaves.position.y = y - 0.45; g.add(leaves); });
  [3, 12, 18].forEach(y => { const r = new THREE.Mesh(new THREE.TorusGeometry(0.95, 0.06, 6, 20), stoneMat); r.rotation.x = Math.PI / 2; r.position.y = y; g.add(r); });
}
[-1, 1].forEach(s => piersZ.forEach(z => pier(s * NAVE, z)));
[-1, 1].forEach(s => [-LEN + 0.6, LEN - 0.6].forEach(z => pier(s * NAVE, z)));

// Volta a sesto acuto con costoloni
const VY = FY + 24, VR = 2 * NAVE;
const vault = new THREE.Mesh(archRing(NAVE, VR, VR + 0.6, 0, LEN * 2), ceilingMat); vault.position.y = VY; vault.receiveShadow = true; staticG.add(vault);
const apexY = VY + archApex(NAVE, VR - 0.7);
[...piersZ, -LEN + 0.6, LEN - 0.6].forEach(z => { const rib = new THREE.Mesh(archRing(NAVE, VR - 0.75, VR + 0.05, 0, 0.7), stoneMat); rib.position.set(0, VY, z); staticG.add(rib);
  const boss = new THREE.Mesh(new THREE.DodecahedronGeometry(0.7), goldMat); boss.position.set(0, apexY - 0.2, z); staticG.add(boss); });
// costoloni diagonali (approssimati) per ogni campata
bayZ.forEach(z => { [-1, 1].forEach(d => { const len = Math.hypot(VR, BAY); const rib = new THREE.Mesh(archRing(NAVE * 0.98, VR - 0.5, VR - 0.2, 0, 0.35), stoneMat);
  rib.position.set(0, VY, z); rib.rotation.y = d * Math.atan2(BAY, VR); rib.scale.x = len / VR; staticG.add(rib); }); });
box(0.4, 0.4, LEN * 2, stoneMat, 0, apexY - 0.05, 0, staticG, false);

// Navate laterali: muri esterni, soffitto, finestre
[-1, 1].forEach(side => {
  const wg = new THREE.PlaneGeometry(LEN * 2, 14); uvScale(wg, LEN * 2 / 4, 14 / 4);
  const w = new THREE.Mesh(wg, wallMat); w.position.set(side * AISLE, FY + 7, 0); w.rotation.y = -side * Math.PI / 2; w.receiveShadow = true; staticG.add(w);
  const cg = new THREE.PlaneGeometry(AISLE - NAVE, LEN * 2); uvScale(cg, 2, 14);
  const c = new THREE.Mesh(cg, ceilingMat); c.rotation.x = Math.PI / 2; c.position.set(side * (NAVE + AISLE) / 2, FY + 13, 0); staticG.add(c);
  bayZ.forEach((z, i) => {
    const g = new THREE.Group(); g.position.set(side * (AISLE - 0.05), FY, z); g.rotation.y = -side * Math.PI / 2; staticG.add(g);
    const gm = new THREE.MeshBasicMaterial({ map: lancetTex(i + 7), side: THREE.DoubleSide, fog: true }); glassMats.push(gm);
    const glass = new THREE.Mesh(fitUV(new THREE.ShapeGeometry(new THREE.Shape(archFill(1.2, 2.4, 2.5, 4.5)))), gm); g.add(glass);
    const fr = new THREE.Mesh(archRing(1.2, 2.4, 2.8, 4.5, 0.5), stoneMat); fr.position.y = 2.5; g.add(fr);
    box(3.0, 0.3, 0.6, stoneMat, 0, 2.4, 0, g, false); box(0.1, 4.6, 0.2, stoneMat, 0, 4.8, 0, g, false);
    // costoloni trasversali della navata laterale
    const rib = new THREE.Mesh(archRing(4, 8, 8.4, 0, 0.5), stoneMat); rib.position.set(side * (NAVE + AISLE) / 2, FY + 5, z - BAY / 2); staticG.add(rib);
  });
});

// Facciata ovest (z = +28): rosone e portale
function endWall(z, dir) {
  const wg = new THREE.PlaneGeometry(AISLE * 2, 42); uvScale(wg, AISLE * 2 / 4, 42 / 4);
  const w = new THREE.Mesh(wg, wallMat); w.position.set(0, FY + 21, z); w.rotation.y = dir > 0 ? Math.PI : 0; w.receiveShadow = true; staticG.add(w);
}
endWall(LEN, 1); endWall(-LEN, -1);
function roseWindow(y, z, r, dir) {
  const g = new THREE.Group(); g.position.set(0, y, z); staticG.add(g);
  const gm = new THREE.MeshBasicMaterial({ map: roseTex, side: THREE.DoubleSide, fog: true }); glassMats.push(gm);
  g.add(new THREE.Mesh(new THREE.CircleGeometry(r, 48), gm));
  const ring = new THREE.Mesh(new THREE.TorusGeometry(r + 0.15, 0.35, 10, 64), stoneMat); ring.position.z = dir * -0.15; g.add(ring);
  const ring2 = new THREE.Mesh(new THREE.TorusGeometry(r * 0.32, 0.12, 8, 32), stoneMat); ring2.position.z = dir * -0.1; g.add(ring2);
  for (let i = 0; i < 16; i++) { const a = i / 16 * Math.PI * 2; const sp = box(0.12, r * 0.68, 0.18, stoneMat, Math.cos(a) * r * 0.66, Math.sin(a) * r * 0.66, dir * -0.1, g, false); sp.rotation.z = a - Math.PI / 2;
    if (i % 2 === 0) { const t = new THREE.Mesh(new THREE.TorusGeometry(r * 0.17, 0.07, 6, 20), stoneMat); t.position.set(Math.cos(a) * r * 0.82, Math.sin(a) * r * 0.82, dir * -0.1); g.add(t); } }
}
roseWindow(FY + 28, LEN - 0.05, 5.2, 1);
roseWindow(FY + 30, -LEN + 0.05, 4.2, -1);
// Portale strombato
{ const g = new THREE.Group(); g.position.set(0, FY, LEN - 0.1); g.rotation.y = Math.PI; staticG.add(g);
  for (let k = 0; k < 5; k++) { const r = new THREE.Mesh(archRing(2.2, 4.4 + k * 0.45, 4.8 + k * 0.45, 5, 0.4), stoneMat); r.position.z = 0.25 + k * 0.05; g.add(r);
    cyl(0.15, 0.15, 5, 8, stoneMat, -(2.4 + k * 0.45), 2.5, 0.35, g); cyl(0.15, 0.15, 5, 8, stoneMat, 2.4 + k * 0.45, 2.5, 0.35, g); }
  const door = new THREE.Mesh(fitUV(new THREE.ShapeGeometry(new THREE.Shape(archFill(2.2, 4.4, 0, 5)))), woodMat); door.position.z = 0.02; g.add(door);
  for (let k = 0; k < 6; k++) box(4.2, 0.12, 0.08, ironMat, 0, 0.8 + k * 1.3, 0.08, g, false);
  box(0.12, 8.6, 0.1, ironMat, 0, 4.3, 0.1, g, false);
}
// Abside: tre lancette alte
[-3.2, 0, 3.2].forEach((x, i) => { const g = new THREE.Group(); g.position.set(x, FY, -LEN + 0.06); staticG.add(g);
  const legH = i === 1 ? 13 : 11, y0 = 6;
  const gm = new THREE.MeshBasicMaterial({ map: lancetTex(i + 20), side: THREE.DoubleSide, fog: true }); glassMats.push(gm);
  g.add(new THREE.Mesh(fitUV(new THREE.ShapeGeometry(new THREE.Shape(archFill(1.1, 2.2, y0, legH)))), gm));
  const fr = new THREE.Mesh(archRing(1.1, 2.2, 2.6, legH, 0.6), stoneMat); fr.position.y = y0; g.add(fr); });

// =====================================================================
// PRESBITERIO: gradini, altare, retablo, stalli del coro
// =====================================================================
for (let i = 0; i < 3; i++) box(NAVE * 2 - 0.6, 0.3, 10 - i * 0.8 + 0.8, stoneMat, 0, FY + 0.15 + i * 0.3, -LEN + 5 - i * 0.4 + 0.4, staticG);
const SY = FY + 0.9; // quota del presbiterio
const altarG = new THREE.Group(); altarG.position.set(0, SY, -24); staticG.add(altarG);
box(4.4, 1.05, 1.7, statueMat, 0, 0.525, 0, altarG);
box(4.7, 0.12, 1.9, statueMat, 0, 1.11, 0, altarG);
for (let k = -2; k <= 2; k++) { const q = new THREE.Group(); q.position.set(k * 0.85, 0.55, 0.87); altarG.add(q);
  for (let j = 0; j < 4; j++) { const t = new THREE.Mesh(new THREE.TorusGeometry(0.14, 0.03, 6, 14), goldMat); const a = j * Math.PI / 2; t.position.set(Math.cos(a) * 0.13, Math.sin(a) * 0.13, 0); q.add(t); } }
box(4.72, 0.46, 1.92, velvetMat, 0, 0.9, 0.01, altarG, false);
box(4.0, 0.02, 1.4, cloth(0xb0a896), 0, 1.19, 0, altarG, false);
// Crocifisso
box(0.12, 1.6, 0.12, goldMat, 0, 2.0, -0.3, altarG); box(0.8, 0.12, 0.12, goldMat, 0, 2.4, -0.3, altarG);
cyl(0.18, 0.28, 0.25, 10, goldMat, 0, 1.3, -0.3, altarG);
for (let i = 0; i < 6; i++) { const x = (i < 3 ? -1 : 1) * (0.6 + (i % 3) * 0.55); cyl(0.07, 0.12, 0.5 - (i % 3) * 0.08, 8, goldMat, x, 1.42 - (i % 3) * 0.04, -0.3, altarG); addCandle(altarG, x, 1.66 - (i % 3) * 0.08, -0.3, 0.55 - (i % 3) * 0.08, 1.1); }
// Retablo con nicchie e pinnacoli
const rer = new THREE.Group(); rer.position.set(0, SY, -27.2); staticG.add(rer);
box(10, 6.5, 0.6, darkStoneMat, 0, 3.25, 0, rer);
[-3.8, -1.9, 0, 1.9, 3.8].forEach((x, i) => { const n = new THREE.Mesh(archRing(0.65, 1.3, 1.5, 2.4, 0.5), goldMat); n.position.set(x, 1.4, 0.35); rer.add(n);
  box(1.3, 3.5, 0.1, new THREE.MeshStandardMaterial({ color: 0x0c0a14, roughness: 1 }), x, 2.9, 0.26, rer, false);
  const st = saintStatue(0.55, i % 2 ? habitBrown : statueMat); st.position.set(x, 1.4, 0.15); rer.add(st);
  box(0.25, 3, 0.25, darkStoneMat, x + 0.95, 7.8, 0.1, rer); const sp = new THREE.Mesh(new THREE.ConeGeometry(0.22, 1.2, 4), goldMat); sp.position.set(x + 0.95, 9.9, 0.1); rer.add(sp); });
{ const sp = new THREE.Mesh(new THREE.ConeGeometry(0.45, 3, 4), goldMat); sp.position.set(0, 9.5, 0.1); rer.add(sp); box(0.6, 3, 0.6, darkStoneMat, 0, 7.2, 0.1, rer); }
for (let i = 0; i < 9; i++) { const gb = new THREE.Mesh(new THREE.ConeGeometry(0.6, 1.2, 3), darkStoneMat); gb.position.set(-4.4 + i * 1.1, 7.1, 0.25); gb.rotation.y = Math.PI / 6; rer.add(gb); }
box(10.4, 0.25, 0.9, goldMat, 0, 6.5, 0.1, rer);

// Stalli del coro
function choirStall(side) {
  const g = new THREE.Group(); g.position.set(side * 6.4, FY, -14.5); g.rotation.y = -side * Math.PI / 2; staticG.add(g);
  box(4.2, 0.5, 0.9, woodMat, 0, 0.75, 0, g); box(4.2, 2.6, 0.12, woodMat, 0, 1.6, -0.45, g);
  for (let i = 0; i <= 4; i++) box(0.08, 1.1, 0.8, woodMat, -2 + i, 1.55, 0, g);
  box(4.4, 0.15, 1.0, woodMat, 0, 3.0, -0.05, g);
  for (let i = 0; i < 5; i++) { const sp = new THREE.Mesh(new THREE.ConeGeometry(0.12, 0.7, 4), woodMat); sp.position.set(-2 + i, 3.4, -0.3); g.add(sp);
    const arc = new THREE.Mesh(archRing(0.25, 0.5, 0.58, 0.4, 0.06), woodMat); arc.position.set(-1.5 + i * 1 - 0.0, 2.1, -0.38); if (i < 4) g.add(arc); }
  box(4.2, 0.9, 0.08, woodMat, 0, 0.95, 0.9, g); box(4.2, 0.06, 0.4, woodMat, 0, 1.42, 0.85, g);
}
choirStall(-1); choirStall(1);

// Banchi
function pew(x, z) {
  const g = new THREE.Group(); g.position.set(x, FY, z); staticG.add(g);
  box(5.6, 0.08, 0.5, woodMat, 0, 0.48, 0, g); box(5.6, 0.75, 0.06, woodMat, 0, 0.95, -0.27, g); box(5.6, 0.08, 0.2, woodMat, 0, 0.18, 0.45, g);
  [-2.85, 2.85].forEach(ex => { const e = new THREE.Mesh(extrude(new THREE.Shape([new V2(-0.32, 0), new V2(0.32, 0), new V2(0.32, 1.25), new V2(0, 1.5), new V2(-0.32, 1.25)]), 0.08), woodMat); e.rotation.y = Math.PI / 2; e.position.x = ex; e.castShadow = true; g.add(e);
    const f = new THREE.Mesh(new THREE.TorusGeometry(0.1, 0.025, 6, 12), woodMat); f.position.set(ex + Math.sign(ex) * 0.05, 1.1, 0); f.rotation.y = Math.PI / 2; g.add(f); });
}
for (let i = 0; i < 6; i++) [-1, 1].forEach(s => pew(s * 4.7, 10.5 + i * 2.5));

// Candelieri in ferro battuto ai lati della pedana
function pricket(x, z, y = FY) {
  const g = new THREE.Group(); g.position.set(x, y, z); staticG.add(g);
  for (let i = 0; i < 3; i++) { const a = i / 3 * Math.PI * 2; const leg = cyl(0.03, 0.04, 0.8, 6, ironMat, Math.cos(a) * 0.25, 0.3, Math.sin(a) * 0.25, g); leg.rotation.z = Math.cos(a) * 0.6; leg.rotation.x = -Math.sin(a) * 0.6; }
  cyl(0.04, 0.05, 2.6, 8, ironMat, 0, 1.9, 0, g);
  [[2.0, 0.45, 5], [2.65, 0.3, 3]].forEach(([yy, r, n]) => { const ring = new THREE.Mesh(new THREE.TorusGeometry(r, 0.025, 6, 24), ironMat); ring.rotation.x = Math.PI / 2; ring.position.y = yy; g.add(ring);
    for (let i = 0; i < n; i++) { const a = i / n * Math.PI * 2; cyl(0.06, 0.02, 0.05, 8, ironMat, Math.cos(a) * r, yy + 0.03, Math.sin(a) * r, g, false); addCandle(g, Math.cos(a) * r, yy + 0.05, Math.sin(a) * r, 0.3 + Math.random() * 0.25, 1.1); } });
  addCandle(g, 0, 3.2, 0, 0.5, 1.3);
}
const PL = FY + 0.7; // quota piano pedana
[[-4.95, -4.95], [4.95, -4.95], [-4.95, 4.95], [4.95, 4.95]].forEach(([x, z]) => pricket(x, z, PL));
pricket(-7.6, -19.6, SY); pricket(7.6, -19.6, SY);

// Corone pendenti
function corona(z) {
  const g = new THREE.Group(); g.position.set(0, FY + 17, z); staticG.add(g);
  [0, 1, 2].forEach(k => { const a = k / 3 * Math.PI * 2; cyl(0.015, 0.015, 24, 4, ironMat, Math.cos(a) * 1.9, 12, Math.sin(a) * 1.9, g, false); });
  const r1 = new THREE.Mesh(new THREE.TorusGeometry(2.0, 0.06, 6, 40), ironMat); r1.rotation.x = Math.PI / 2; g.add(r1);
  const r2 = new THREE.Mesh(new THREE.TorusGeometry(2.0, 0.04, 6, 40), ironMat); r2.rotation.x = Math.PI / 2; r2.position.y = 0.35; g.add(r2);
  for (let i = 0; i < 16; i++) { const a = i / 16 * Math.PI * 2; cyl(0.015, 0.015, 0.35, 4, ironMat, Math.cos(a) * 2, 0.17, Math.sin(a) * 2, g, false);
    if (i % 2) { const f = new THREE.Mesh(new THREE.ConeGeometry(0.06, 0.3, 4), ironMat); f.position.set(Math.cos(a) * 2, 0.55, Math.sin(a) * 2); g.add(f); }
    else addCandle(g, Math.cos(a) * 2, 0.36, Math.sin(a) * 2, 0.28, 1.2); }
  const l = new THREE.PointLight(0xffa04a, 1.1, 18, 2); l.position.y = -0.4; g.add(l); flickerLights.push({ light: l, base: 1.1, phase: z, optional: true });
}
corona(-16); corona(17);

// =====================================================================
// STATUE E SCULTURE (statiche)
// =====================================================================
// ---------- Vento nei mantelli: i tessuti ondeggiano (deformazione nel vertex shader, costo quasi nullo) ----------
const CLOTH_T = { value: 0 };
const swayCache = new Map();
function swayMat(mat, amp = 1) {
  const key = mat.uuid + amp; if (swayCache.has(key)) return swayCache.get(key);
  const m = mat.clone();
  m.onBeforeCompile = sh => { sh.uniforms.uClothT = CLOTH_T;
    sh.vertexShader = 'uniform float uClothT;\n' + sh.vertexShader.replace('#include <begin_vertex>', `#include <begin_vertex>
      vec3 sw0 = (modelMatrix * vec4(0.0, 0.0, 0.0, 1.0)).xyz; float swPh = sw0.x * 1.7 + sw0.z * 1.1;
      float swH = clamp(1.0 - position.y / 2.1, 0.0, 1.0); swH *= swH;
      transformed.x += sin(uClothT * 1.6 + swPh + position.y * 2.6 + position.z * 3.0) * 0.03 * ${amp.toFixed(2)} * swH;
      transformed.z += cos(uClothT * 1.25 + swPh + position.y * 2.1 + position.x * 3.0) * 0.024 * ${amp.toFixed(2)} * swH;`); };
  m.customProgramCacheKey = () => 'sway' + amp; swayCache.set(key, m); return m;
}
// ---------- Maghi: mantello aperto, veste interna, cappuccio profondo con occhi luminosi, mani, bastone con cristallo ----------
const skinGaunt = new THREE.MeshStandardMaterial({ color: 0x8c7b70, roughness: 0.62 });
const staffWood = new THREE.MeshStandardMaterial({ map: woodSet.map, normalMap: woodSet.normalMap, color: 0x5a4030, roughness: 0.7 });
const leatherMat = new THREE.MeshStandardMaterial({ color: 0x2a1a10, roughness: 0.75 });
const silverMat = new THREE.MeshStandardMaterial({ color: 0xb8bcc4, roughness: 0.3, metalness: 0.7 });
function makeCloak(h, r, open, folds = 12, y0 = 0, y1 = 0.94) {
  const prof = [[r * 1.08, h * y0], [r * 1.05, h * (y0 + 0.02)], [r * 0.96, h * 0.3], [r * 0.82, h * 0.55], [r * 0.68, h * 0.74], [r * 0.6, h * 0.82], [r * 0.44, h * 0.9], [r * 0.22, h * y1]].filter(p => p[1] >= h * y0 - 1e-6);
  const g = new THREE.LatheGeometry(prof.map(p => new V2(p[0], p[1])), IS_MOBILE ? 40 : 64, open, Math.PI * 2 - open * 2), pos = g.attributes.position;
  for (let i = 0; i < pos.count; i++) { const x = pos.getX(i), y = pos.getY(i), z = pos.getZ(i), a = Math.atan2(x, z), f = 1 - y / h;
    const k = 1 + (Math.sin(a * folds) * 0.6 + Math.sin(a * folds * 2.13 + 1.3) * 0.4) * 0.11 * f;
    pos.setXYZ(i, x * k, y + (y < h * 0.05 ? Math.sin(a * 5) * 0.012 : 0), z * k); }
  g.computeVertexNormals(); uvScale(g, 3, 2); return g;
}
function makeHand(skin = skinGaunt) {
  const g = new THREE.Group();
  const palm = new THREE.Mesh(new THREE.SphereGeometry(1, 12, 10), skin); palm.scale.set(0.042, 0.055, 0.022); g.add(palm);
  for (let i = 0; i < 4; i++) { const f = new THREE.Mesh(new THREE.CylinderGeometry(0.0085, 0.007, 0.06, 6), skin); f.position.set(-0.027 + i * 0.018, -0.075, 0.004); f.rotation.x = 0.35 + i * 0.05; g.add(f);
    const tip = new THREE.Mesh(new THREE.SphereGeometry(0.0072, 6, 4), skin); tip.position.set(-0.027 + i * 0.018, -0.105, 0.016 + i * 0.002); g.add(tip); }
  const th = new THREE.Mesh(new THREE.CylinderGeometry(0.009, 0.008, 0.05, 6), skin); th.position.set(0.045, -0.03, 0.015); th.rotation.z = 0.8; g.add(th);
  return g;
}
const runeTex = (() => { const c = mkCanvas(64), g = c.getContext('2d'); g.strokeStyle = '#fff'; g.lineWidth = 4; g.lineCap = 'round'; g.shadowColor = '#fff'; g.shadowBlur = 6;
  const glyphs = [[[16, 10, 16, 54], [16, 22, 44, 12], [16, 34, 44, 24]], [[32, 8, 32, 56], [14, 20, 50, 44], [50, 20, 14, 44]], [[18, 10, 18, 54], [18, 10, 46, 30], [46, 30, 18, 46]], [[14, 54, 32, 10], [32, 10, 50, 54], [20, 38, 44, 38]]];
  const tex = []; glyphs.forEach(gl => { const cc = mkCanvas(64), gg = cc.getContext('2d'); gg.strokeStyle = '#fff'; gg.lineWidth = 4; gg.lineCap = 'round'; gg.shadowColor = '#fff'; gg.shadowBlur = 8;
    gl.forEach(([a, b, c2, d]) => { gg.beginPath(); gg.moveTo(a, b); gg.lineTo(c2, d); gg.stroke(); }); tex.push(new THREE.CanvasTexture(cc)); }); return tex; })();
function mageFigure({ h = 1.95, r = 0.5, outer, inner, trim = goldMat, eye = new THREE.Color(0.6, 1.4, 3.0), crystal = new THREE.Color(1.6, 2.4, 3.2), staff = true, runes = false, light = 0 }) {
  const g = new THREE.Group();
  const robe = new THREE.Mesh(makeRobe(h * 0.985, r * 0.86, 10), swayMat(inner, 0.6)); robe.castShadow = true; robe.receiveShadow = true; g.add(robe);
  const cloak = new THREE.Mesh(makeCloak(h, r, 0.42), swayMat(outer, 1)); cloak.castShadow = true; cloak.receiveShadow = true; g.add(cloak);
  const hem = new THREE.Mesh(new THREE.LatheGeometry([[r * 1.095, 0.005], [r * 1.09, 0.06]].map(p => new V2(p[0], p[1])), 64, 0.42, Math.PI * 2 - 0.84), swayMat(trim, 1)); g.add(hem);
  [-1, 1].forEach(s => { const edge = new THREE.Mesh(new THREE.BoxGeometry(0.035, h * 0.86, 0.012), trim); const a = s * 0.42, rr = r * 0.82;
    edge.position.set(Math.sin(a) * rr, h * 0.45, Math.cos(a) * rr + 0.03); edge.rotation.x = -0.18; edge.rotation.y = a; g.add(edge); });
  const mantle = new THREE.Mesh(makeCloak(h, r * 1.02, 0.6, 9, 0.68, 0.92), outer); mantle.castShadow = true; g.add(mantle);
  const sh = new THREE.Mesh(new THREE.SphereGeometry(1, 20, 12), outer); sh.scale.set(r * 0.82, r * 0.36, r * 0.62); sh.position.y = h * 0.83; sh.castShadow = true; g.add(sh);
  const head = new THREE.Group(); head.position.y = h * 0.935; g.add(head); const cr = r * 0.56;
  const hood = new THREE.Mesh(new THREE.SphereGeometry(cr, 28, 18, Math.PI / 2 + 0.55, Math.PI * 2 - 1.1, 0, Math.PI * 0.82), outer); hood.scale.set(1, 1.18, 1.18); hood.castShadow = true; head.add(hood);
  const tip = new THREE.Mesh(new THREE.ConeGeometry(cr * 0.5, cr * 2.1, 12), outer); tip.position.set(0, cr * 0.25, -cr * 1.05); tip.rotation.x = -1.25; head.add(tip);
  const face = new THREE.Mesh(new THREE.SphereGeometry(cr * 0.8, 16, 12), darkFaceMat); face.position.set(0, -cr * 0.06, cr * 0.04); head.add(face);
  const eyeMat = new THREE.MeshBasicMaterial({ color: eye.clone() });
  [-1, 1].forEach(s => { const e = new THREE.Mesh(new THREE.SphereGeometry(cr * 0.075, 10, 8), eyeMat); e.scale.set(1.3, 0.75, 0.6); e.position.set(s * cr * 0.25, -cr * 0.02, cr * 0.8); head.add(e); });
  const belt = new THREE.Mesh(new THREE.TorusGeometry(r * 0.66, 0.03, 8, 32), leatherMat); belt.rotation.x = Math.PI / 2; belt.position.y = h * 0.5; belt.scale.set(1, 0.82, 1); g.add(belt);
  const buckle = new THREE.Mesh(new THREE.BoxGeometry(0.07, 0.06, 0.02), trim); buckle.position.set(0, h * 0.5, r * 0.56); g.add(buckle);
  const pouch = new THREE.Mesh(new THREE.BoxGeometry(0.1, 0.12, 0.06), leatherMat); pouch.position.set(r * 0.42, h * 0.45, r * 0.42); pouch.rotation.y = 0.8; g.add(pouch);
  const amul = new THREE.Mesh(new THREE.TorusGeometry(0.045, 0.008, 6, 20), trim); amul.position.set(0, h * 0.72, r * 0.52); g.add(amul);
  const gemMat = new THREE.MeshBasicMaterial({ color: crystal.clone().multiplyScalar(0.7) });
  const gem = new THREE.Mesh(new THREE.OctahedronGeometry(0.028), gemMat); gem.position.copy(amul.position); gem.position.z += 0.005; g.add(gem);
  const chain = new THREE.Mesh(new THREE.TorusGeometry(r * 0.32, 0.004, 4, 24, Math.PI), trim); chain.position.set(0, h * 0.8, r * 0.3); chain.rotation.x = 0.5; chain.rotation.z = Math.PI; g.add(chain);
  function arm(side) { const p = new THREE.Group(); p.position.set(side * r * 0.56, h * 0.82, 0.02); g.add(p);
    const sl = new THREE.Mesh(new THREE.CylinderGeometry(r * 0.13, r * 0.27, h * 0.37, 18, 1, true), swayMat(outer, 0.4)); sl.position.y = -h * 0.185; sl.castShadow = true; p.add(sl);
    const cuff = new THREE.Mesh(new THREE.TorusGeometry(r * 0.27, 0.012, 6, 22), trim); cuff.rotation.x = Math.PI / 2; cuff.position.y = -h * 0.37; p.add(cuff);
    const hand = makeHand(); hand.position.y = -h * 0.39; hand.rotation.y = side * 0.3; p.add(hand); return { p, hand }; }
  const armR = arm(1), armL = arm(-1);
  armR.p.rotation.set(-0.45, 0, 0.08); armL.p.rotation.set(-0.5, 0, -0.06);
  let top = null, crystalMat = null, glow = null, pl = null;
  if (staff) {
    const sx = -r * 0.95, sz = r * 0.3, H = h * 1.13;
    const path = new THREE.CatmullRomCurve3([[0, 0, 0], [0.02, H * 0.25, 0.01], [-0.018, H * 0.5, -0.01], [0.022, H * 0.75, 0.012], [-0.01, H * 0.94, 0], [0.01, H, 0.0]].map(p => new THREE.Vector3(...p)));
    const st = new THREE.Mesh(new THREE.TubeGeometry(path, 40, 0.022, 8, false), staffWood); st.position.set(sx, 0, sz); st.castShadow = true; g.add(st);
    [0.3, 0.55].forEach(t => { const kn = new THREE.Mesh(new THREE.SphereGeometry(0.03, 8, 6), staffWood); kn.position.copy(path.getPoint(t)).add(new THREE.Vector3(sx, 0, sz)); kn.scale.set(1, 1.6, 1); g.add(kn); });
    top = new THREE.Group(); top.position.set(sx + 0.01, H + 0.09, sz); g.add(top);
    for (let i = 0; i < 4; i++) { const a = i / 4 * Math.PI * 2; const cl = new THREE.Mesh(new THREE.ConeGeometry(0.014, 0.17, 6), staffWood); cl.position.set(Math.cos(a) * 0.045, -0.02, Math.sin(a) * 0.045); cl.rotation.set(Math.sin(a) * -0.45, 0, Math.cos(a) * 0.45); top.add(cl); }
    crystalMat = new THREE.MeshBasicMaterial({ color: crystal.clone() });
    const cry = new THREE.Mesh(new THREE.OctahedronGeometry(0.05, 0), crystalMat); cry.scale.set(1, 1.9, 1); cry.position.y = 0.03; top.add(cry); top.userData.cry = cry;
    glow = makeGlow(new THREE.Color(crystal.r / 3.2, crystal.g / 3.2, crystal.b / 3.2), 0.9, 0.55); glow.position.y = 0.03; top.add(glow);
    if (light) { pl = new THREE.PointLight(new THREE.Color(crystal.r / 3.2, crystal.g / 3.2, crystal.b / 3.2), light, 7, 2); pl.position.y = 0.05; top.add(pl); }
    armL.p.rotation.set(-0.3, 0, -0.55);
  }
  let runeG = null;
  if (runes) { runeG = new THREE.Group(); runeG.position.y = h * 0.55; g.add(runeG);
    for (let i = 0; i < 8; i++) { const a = i / 8 * Math.PI * 2; const s = new THREE.Sprite(new THREE.SpriteMaterial({ map: runeTex[i % 4], color: new THREE.Color(crystal.r / 3.2, crystal.g / 3.2, crystal.b / 3.2), transparent: true, opacity: 0.2, blending: THREE.AdditiveBlending, depthWrite: false }));
      s.scale.setScalar(0.22); s.position.set(Math.cos(a) * r * 1.9, 0, Math.sin(a) * r * 1.9); s.renderOrder = 6; runeG.add(s); } }
  return { g, head, robe, cloak, armR, armL, top, crystalMat, glow, pl, eyeMat, runeG, h, baseEye: eye.clone(), baseCry: crystal.clone() };
}

function makeRobe(h, r, folds = 9, tatter = 0) {
  const prof = [[0.001, 0], [r, 0], [r * 0.98, 0.05], [r * 0.88, h * 0.3], [r * 0.73, h * 0.55], [r * 0.6, h * 0.74], [r * 0.56, h * 0.82], [r * 0.38, h * 0.9], [0.001, h * 0.93]];
  const g = new THREE.LatheGeometry(prof.map(p => new V2(p[0], p[1])), IS_MOBILE ? 40 : 56), pos = g.attributes.position;
  for (let i = 0; i < pos.count; i++) { const x = pos.getX(i), y = pos.getY(i), z = pos.getZ(i), a = Math.atan2(z, x), f = 1 - y / h;
    const k = 1 + (Math.sin(a * folds) * 0.6 + Math.sin(a * folds * 2.3 + 1) * 0.4) * 0.09 * f; let yy = y;
    if (tatter && y < h * 0.12) yy = y + Math.abs(Math.sin(a * 11)) * tatter + Math.random() * tatter * 0.3;
    pos.setXYZ(i, x * k, yy, z * k); }
  g.computeVertexNormals(); uvScale(g, 3, 2); return g;
}
function hoodedFigure({ h = 1.9, r = 0.5, mat, faceMat = darkFaceMat, tatter = 0, sleeves = true }) {
  const g = new THREE.Group();
  const robe = new THREE.Mesh(makeRobe(h, r, 9, tatter), (mat === statueMat || mat.transparent) ? mat : swayMat(mat)); robe.castShadow = true; robe.receiveShadow = true; g.add(robe);
  const sh = new THREE.Mesh(new THREE.SphereGeometry(1, 18, 12), mat); sh.scale.set(r * 0.8, r * 0.34, r * 0.6); sh.position.y = h * 0.83; sh.castShadow = true; g.add(sh);
  const drape = new THREE.Mesh(new THREE.ConeGeometry(r * 0.78, h * 0.2, 24, 1, true), mat); drape.position.y = h * 0.82; drape.castShadow = true; g.add(drape);
  const head = new THREE.Group(); head.position.y = h * 0.93; g.add(head);
  const cr = r * 0.55;
  const cowl = new THREE.Mesh(new THREE.SphereGeometry(cr, 22, 14, Math.PI / 2 + 0.6, Math.PI * 2 - 1.2, 0, Math.PI * 0.8), mat); cowl.scale.set(1, 1.15, 1.1); cowl.castShadow = true; head.add(cowl);
  const tip = new THREE.Mesh(new THREE.ConeGeometry(cr * 0.55, cr * 1.7, 10), mat); tip.position.set(0, cr * 0.35, -cr * 0.85); tip.rotation.x = -1.15; head.add(tip);
  const face = new THREE.Mesh(new THREE.SphereGeometry(cr * 0.8, 14, 10), faceMat); face.position.set(0, -cr * 0.05, cr * 0.05); head.add(face);
  let sl = null;
  if (sleeves) { sl = new THREE.Mesh(new THREE.CylinderGeometry(r * 0.26, r * 0.36, r * 1.3, 14), mat); sl.rotation.z = Math.PI / 2; sl.position.set(0, h * 0.58, r * 0.6); sl.castShadow = true; g.add(sl); }
  const belt = new THREE.Mesh(new THREE.TorusGeometry(r * 0.78, 0.025, 6, 28), ropeMat); belt.rotation.x = Math.PI / 2; belt.position.y = h * 0.5; g.add(belt);
  const cord = new THREE.Mesh(new THREE.CylinderGeometry(0.016, 0.016, h * 0.32, 5), ropeMat); cord.position.set(r * 0.3, h * 0.33, r * 0.82); g.add(cord);
  return { g, head, robe, sleeves: sl, h };
}
function saintStatue(scale = 1, mat = statueMat) {
  const f = hoodedFigure({ h: 2.0, r: 0.42, mat: statueMat, faceMat: statueMat });
  const book = new THREE.Mesh(new THREE.BoxGeometry(0.32, 0.4, 0.07), statueMat); book.position.set(0, 1.25, 0.42); book.rotation.x = -0.4; f.g.add(book);
  const halo = new THREE.Mesh(new THREE.CylinderGeometry(0.34, 0.34, 0.04, 24), statueMat); halo.rotation.x = Math.PI / 2; halo.position.set(0, 1.95, -0.2); f.g.add(halo);
  f.g.scale.setScalar(scale); return f.g;
}
// Santi su mensole con baldacchino, sui pilastri
[-1, 1].forEach(side => piersZ.forEach(z => {
  const g = new THREE.Group(); g.position.set(side * (NAVE - 1.05), FY, z); g.rotation.y = -side * Math.PI / 2; staticG.add(g);
  const corbel = lathe([[0.001, 0], [0.12, 0], [0.4, 0.5], [0.5, 0.8], [0.001, 0.8]], 8, stoneMat); corbel.position.y = 1.6; g.add(corbel);
  const st = saintStatue(1); st.position.y = 2.4; g.add(st);
  box(1.0, 0.12, 0.9, stoneMat, 0, 4.6, 0, g);
  const can = new THREE.Mesh(new THREE.ConeGeometry(0.62, 1.4, 4), stoneMat); can.position.y = 5.35; can.rotation.y = Math.PI / 4; g.add(can);
  [-0.42, 0.42].forEach(x => { const p = new THREE.Mesh(new THREE.ConeGeometry(0.07, 0.5, 4), stoneMat); p.position.set(x, 4.9, 0.35); g.add(p); });
  const ar = new THREE.Mesh(archRing(0.25, 0.5, 0.58, 0.1, 0.08), stoneMat); ar.position.set(0, 4.2, 0.42); ar.scale.set(1.6, 0.8, 1); g.add(ar);
}));

// Angeli piangenti in ginocchio ai lati dei gradini
function weepingAngel(x, z, rot) {
  const g = new THREE.Group(); g.position.set(x, SY, z); g.rotation.y = rot; staticG.add(g);
  box(1.4, 0.9, 1.4, stoneMat, 0, 0.45, 0, g); box(1.55, 0.12, 1.55, stoneMat, 0, 0.95, 0, g);
  const body = new THREE.Mesh(makeRobe(1.35, 0.55, 11), statueMat); body.position.y = 1.0; body.castShadow = true; g.add(body);
  const head = new THREE.Mesh(new THREE.SphereGeometry(0.17, 16, 12), statueMat); head.position.set(0, 2.18, 0.16); g.add(head);
  const hair = new THREE.Mesh(new THREE.SphereGeometry(0.19, 14, 10, 0, Math.PI * 2, 0, Math.PI * 0.6), statueMat); hair.position.copy(head.position); hair.position.z -= 0.03; hair.rotation.x = 0.5; g.add(hair);
  [-1, 1].forEach(s => { const arm = cyl(0.06, 0.08, 0.55, 8, statueMat, s * 0.17, 2.0, 0.22, g); arm.rotation.x = -0.9; arm.rotation.z = s * 0.35;
    const hand = new THREE.Mesh(new THREE.SphereGeometry(0.07, 8, 6), statueMat); hand.position.set(s * 0.07, 2.2, 0.32); g.add(hand); });
  [-1, 1].forEach(s => { for (let i = 0; i < 9; i++) { const f = new THREE.Mesh(new THREE.SphereGeometry(1, 10, 8), statueMat); const l = 0.55 + i * 0.09;
    f.scale.set(0.09, l, 0.025); f.position.set(s * (0.2 + i * 0.05), 1.75 + Math.sin(i / 8 * Math.PI) * 0.35 - i * 0.03, -0.25 - i * 0.03); f.rotation.z = s * (-0.15 - i * 0.06); f.rotation.y = s * 0.4; f.castShadow = true; g.add(f); } });
}
weepingAngel(-7.6, -21.5, Math.PI * 0.1); weepingAngel(7.6, -21.5, -Math.PI * 0.1);

// Doccioni (chimere) sopra le arcate
const gargoyleEyes = [];
function gargoyle(x, z, side) {
  const g = new THREE.Group(); g.position.set(x, FY + 21.4, z); g.rotation.y = -side * Math.PI / 2; staticG.add(g);
  box(1.2, 0.3, 1.2, stoneMat, 0, -0.15, 0, g);
  const body = new THREE.Mesh(new THREE.SphereGeometry(0.5, 14, 10), stoneMat); body.scale.set(0.9, 0.85, 1.1); body.position.set(0, 0.45, 0); g.add(body);
  [-1, 1].forEach(s => { const leg = new THREE.Mesh(new THREE.SphereGeometry(0.25, 10, 8), stoneMat); leg.scale.set(0.8, 1, 1.3); leg.position.set(s * 0.35, 0.2, 0.2); g.add(leg);
    const claw = new THREE.Mesh(new THREE.ConeGeometry(0.06, 0.2, 4), stoneMat); claw.rotation.x = Math.PI / 2; claw.position.set(s * 0.35, 0.05, 0.55); g.add(claw);
    const ws = new THREE.Shape([new V2(0, 0), new V2(0.9, 0.5), new V2(0.8, 0.1), new V2(1.0, -0.1), new V2(0.6, -0.15), new V2(0.55, -0.35), new V2(0.25, -0.15)]);
    const w = new THREE.Mesh(extrude(ws, 0.04), stoneMat); w.position.set(s * 0.2, 0.7, -0.3); w.rotation.y = s > 0 ? -0.4 : Math.PI + 0.4; w.rotation.z = 0.3; g.add(w);
    const horn = new THREE.Mesh(new THREE.ConeGeometry(0.06, 0.35, 6), stoneMat); horn.position.set(s * 0.15, 1.25, 0.25); horn.rotation.z = -s * 0.5; horn.rotation.x = -0.4; g.add(horn); });
  const head = new THREE.Mesh(new THREE.SphereGeometry(0.3, 12, 10), stoneMat); head.position.set(0, 0.95, 0.45); g.add(head);
  const snout = box(0.28, 0.2, 0.35, stoneMat, 0, 0.85, 0.75, g, false);
  const jaw = box(0.24, 0.07, 0.3, stoneMat, 0, 0.7, 0.72, g, false); jaw.rotation.x = 0.3;
  [-1, 1].forEach(s => { const e = makeGlow(0xff2a10, 0.22, 0.6); e.position.set(s * 0.1, 0.98, 0.72); g.add(e); gargoyleEyes.push(e); });
}
[[-1, -12], [-1, 12], [1, -12], [1, 12], [-1, -4], [1, 4]].forEach(([s, z]) => gargoyle(s * (NAVE - 1.1), z, s));

// Tombe con cavaliere giacente
function tomb(x, z) {
  const g = new THREE.Group(); g.position.set(x, FY, z); staticG.add(g);
  box(1.4, 1.0, 2.8, darkStoneMat, 0, 0.5, 0, g); box(1.6, 0.15, 3.0, stoneMat, 0, 1.08, 0, g); box(1.6, 0.15, 3.0, stoneMat, 0, 0.07, 0, g);
  [-1, 1].forEach(s => { for (let k = -1; k <= 1; k++) { const q = new THREE.Group(); q.position.set(s * 0.71, 0.55, k * 0.85); q.rotation.y = Math.PI / 2; g.add(q);
    for (let j = 0; j < 4; j++) { const t = new THREE.Mesh(new THREE.TorusGeometry(0.11, 0.025, 6, 12), stoneMat); const a = j * Math.PI / 2; t.position.set(Math.cos(a) * 0.1, Math.sin(a) * 0.1, 0); q.add(t); } } });
  const k = new THREE.Group(); k.position.y = 1.15; g.add(k);
  const body = new THREE.Mesh(makeRobe(1.7, 0.36, 7), statueMat); body.rotation.x = Math.PI / 2; body.position.set(0, 0.25, -0.95); body.scale.set(1, 1, 0.55); k.add(body);
  const helm = new THREE.Mesh(new THREE.SphereGeometry(0.17, 14, 10), statueMat); helm.position.set(0, 0.25, 0.95); k.add(helm);
  box(0.2, 0.06, 0.22, statueMat, 0, 0.3, 1.12, k, false);
  box(0.4, 0.15, 0.18, statueMat, 0, 0.17, 1.35, k, false);
  const sword = box(0.06, 0.05, 1.2, statueMat, 0, 0.45, -0.1, k, false); box(0.35, 0.06, 0.06, statueMat, 0, 0.47, 0.42, k, false);
  const hands = new THREE.Mesh(new THREE.SphereGeometry(0.09, 8, 6), statueMat); hands.position.set(0, 0.48, 0.5); hands.scale.set(1, 0.8, 1.5); k.add(hands);
  const lion = new THREE.Mesh(new THREE.SphereGeometry(0.2, 10, 8), statueMat); lion.position.set(0, 0.15, -1.25); lion.scale.set(1.2, 0.8, 0.9); k.add(lion);
  addCandle(g, 0.55, 1.15, 1.25, 0.25, 0.9);
}
tomb(-13, -8); tomb(13, 8); tomb(13, -18);

// Pedana della scacchiera
box(11, 1.3, 11, darkStoneMat, 0, FY + 0.65 - 0.6, 0, staticG);
box(12.4, 0.3, 12.4, stoneMat, 0, FY + 0.15, 0, staticG);
box(11.2, 0.1, 11.2, goldMat, 0, PL - 0.03, 0, staticG, false);
[-1, 1].forEach(s => [-1, 1].forEach(t => { const c = cyl(0.3, 0.35, 1.4, 8, stoneMat, s * 5.5, FY + 0.7, t * 5.5, staticG); }));

// =====================================================================
// LUCI
// =====================================================================
const ambientLight = new THREE.AmbientLight(0x1c1724, 0.35); scene.add(ambientLight);
const hemi = new THREE.HemisphereLight(0x2a2840, 0x0a0806, 0.25); scene.add(hemi);
const moon = new THREE.DirectionalLight(0x7d8fbf, 0.25); moon.position.set(32, 42, 10); moon.target.position.set(-2, FY, 0);
moon.castShadow = true; moon.shadow.mapSize.set(2048, 2048); Object.assign(moon.shadow.camera, { left: -24, right: 24, top: 30, bottom: -30, near: 10, far: 110 }); moon.shadow.bias = -0.0004; moon.shadow.normalBias = 0.06;
scene.add(moon, moon.target);
const boardSpot = new THREE.SpotLight(0xffc27a, 3.4, 34, 0.42, 0.55, 1.1); boardSpot.position.set(0, 17, 1.5); boardSpot.target.position.set(0, 0, 0);
boardSpot.castShadow = true; boardSpot.shadow.mapSize.set(2048, 2048); boardSpot.shadow.camera.near = 6; boardSpot.shadow.camera.far = 30; boardSpot.shadow.bias = -0.0003; boardSpot.shadow.normalBias = 0.02;
scene.add(boardSpot, boardSpot.target);
[[-4.95, -4.95], [4.95, -4.95], [-4.95, 4.95], [4.95, 4.95]].forEach(([x, z], i) => { const l = new THREE.PointLight(0xff9a40, 0.8, 9, 2); l.position.set(x, PL + 3.1, z); scene.add(l); flickerLights.push({ light: l, base: 0.8, phase: i * 2.1 }); });
const altarLight = new THREE.PointLight(0xffa850, 1.5, 14, 2); altarLight.position.set(0, SY + 2.3, -23.4); scene.add(altarLight); flickerLights.push({ light: altarLight, base: 1.5, phase: 3 });
const reredosLight = new THREE.SpotLight(0xffb070, 1.2, 16, 0.6, 0.8, 1.5); reredosLight.position.set(0, SY + 9, -20); reredosLight.target.position.set(0, SY + 3, -27); scene.add(reredosLight, reredosLight.target);

// =====================================================================
// FASCI DI LUCE, PULVISCOLO, INCENSO
// =====================================================================
const shaftG = new THREE.Group(); scene.add(shaftG); const shafts = [];
const shaftFade = (() => { const c = mkCanvas(64), g = c.getContext('2d'); const gr = g.createLinearGradient(0, 64, 0, 0); gr.addColorStop(0, '#000'); gr.addColorStop(0.25, '#555'); gr.addColorStop(0.8, '#fff'); gr.addColorStop(1, '#000'); g.fillStyle = gr; g.fillRect(0, 0, 64, 64); return new THREE.CanvasTexture(c); })();
function addShaft(from, to, r1, r2, color, op) {
  const len = from.distanceTo(to);
  const m = new THREE.Mesh(new THREE.CylinderGeometry(r1, r2, len, 24, 1, true), new THREE.MeshBasicMaterial({ color, alphaMap: shaftFade, transparent: true, opacity: op, blending: THREE.AdditiveBlending, depthWrite: false, side: THREE.FrontSide, fog: true }));
  m.position.copy(from).add(to).multiplyScalar(0.5); m.quaternion.setFromUnitVectors(new THREE.Vector3(0, 1, 0), from.clone().sub(to).normalize()); m.renderOrder = 5;
  shaftG.add(m); shafts.push({ mesh: m, base: op, phase: Math.random() * 6 });
}
bayZ.forEach(z => addShaft(new THREE.Vector3(NAVE - 0.3, FY + 17, z), new THREE.Vector3(-1.5, FY, z + 1.2), 1.2, 3.2, 0xbfd0ff, 0.03));
addShaft(new THREE.Vector3(0, FY + 28, -LEN + 1), new THREE.Vector3(0, FY + 1, -17), 2.5, 4.5, 0xc0a0ff, 0.022);
addShaft(new THREE.Vector3(0, 16.5, 1.4), new THREE.Vector3(0, 0, 0), 0.4, 4.8, 0xffd7a0, 0.03);

const dustCount = 1100, dustGeo = new THREE.BufferGeometry(), dustPos = new Float32Array(dustCount * 3);
for (let i = 0; i < dustCount; i++) { dustPos[i * 3] = (Math.random() - 0.5) * 32; dustPos[i * 3 + 1] = FY + Math.random() * 32; dustPos[i * 3 + 2] = (Math.random() - 0.5) * 54; }
dustGeo.setAttribute('position', new THREE.BufferAttribute(dustPos, 3));
const dust = new THREE.Points(dustGeo, new THREE.PointsMaterial({ size: 0.05, color: 0xc8b090, map: glowTex, transparent: true, opacity: 0.55, blending: THREE.AdditiveBlending, depthWrite: false }));
scene.add(dust);
const smokeG = new THREE.Group(); scene.add(smokeG); const smoke = [];
function smokePuff() { const s = new THREE.Sprite(new THREE.SpriteMaterial({ map: smokeTex, color: 0x8a8090, transparent: true, opacity: 0, depthWrite: false, fog: true })); smokeG.add(s); return s; }
for (let i = 0; i < 70; i++) smoke.push({ s: smokePuff(), t: Math.random(), src: i % 2, dx: Math.random() - 0.5 });

// =====================================================================
// PERSONAGGI ANIMATI
// =====================================================================
const figuresG = new THREE.Group(); scene.add(figuresG);
const creaturesG = new THREE.Group(); scene.add(creaturesG);

function lantern() {
  const g = new THREE.Group();
  for (let i = 0; i < 4; i++) { const a = i / 4 * Math.PI * 2 + Math.PI / 4; const p = new THREE.Mesh(new THREE.CylinderGeometry(0.008, 0.008, 0.22, 4), ironMat); p.position.set(Math.cos(a) * 0.07, 0, Math.sin(a) * 0.07); g.add(p); }
  const top = new THREE.Mesh(new THREE.ConeGeometry(0.11, 0.12, 4), ironMat); top.position.y = 0.17; top.rotation.y = Math.PI / 4; g.add(top);
  const bot = new THREE.Mesh(new THREE.BoxGeometry(0.15, 0.025, 0.15), ironMat); bot.position.y = -0.11; g.add(bot);
  const ring = new THREE.Mesh(new THREE.TorusGeometry(0.04, 0.008, 4, 10), ironMat); ring.position.y = 0.26; g.add(ring);
  const pane = new THREE.Mesh(new THREE.BoxGeometry(0.12, 0.2, 0.12), new THREE.MeshBasicMaterial({ color: 0x5a3010, fog: true })); g.add(pane);
  const fl = new THREE.Mesh(new THREE.SphereGeometry(1, 8, 6), flameMat); fl.scale.set(0.02, 0.05, 0.02); g.add(fl);
  g.remove(fl); const fw = new THREE.Group(); fw.add(fl); g.add(fw);
  const glow = makeGlow(0xff9a3c, 1.2, 0.8); g.add(glow);
  flames.push({ fg: fw, glow, base: 1.2, phase: Math.random() * 9 });
  return g;
}
function censer() {
  const pivot = new THREE.Group();
  const chain = new THREE.Mesh(new THREE.CylinderGeometry(0.006, 0.006, 0.7, 4), goldMat); chain.position.y = -0.35; pivot.add(chain);
  const bowl = new THREE.Group(); bowl.position.y = -0.75; pivot.add(bowl);
  bowl.add(new THREE.Mesh(new THREE.SphereGeometry(0.09, 12, 8, 0, Math.PI * 2, Math.PI / 2, Math.PI / 2), bronzeMat));
  const lid = new THREE.Mesh(new THREE.ConeGeometry(0.09, 0.14, 12), bronzeMat); lid.position.y = 0.07; bowl.add(lid);
  const ember = makeGlow(0xff5a20, 0.35, 0.9); ember.position.y = 0.03; bowl.add(ember);
  return { pivot, bowl };
}

// Processione dei monaci attorno alla pedana
const procPath = new THREE.CatmullRomCurve3([
  [14.8, -8.0], [14.9, 8], [14.8, 24.4], [7, 24.9], [0, 25.0], [-7, 24.9], [-14.8, 24.4], [-14.9, 8], [-14.8, -8.0], [-7, -8.3], [0, -8.4], [7, -8.3]
].map(([x, z]) => new THREE.Vector3(x, FY, z)), true, 'centripetal');
const monks = []; let censerSmokeSrc = new THREE.Vector3();
const PROC = [[habitBrown, 0x2a2a30], [habitBlack, 0x30222a], [habitBrown, 0x1c2232], [habitBlack, 0x2a2a30], [habitBrown, 0x2c2420], [habitBlack, 0x1c2232]];
for (let i = 0; i < 6; i++) {
  const f = mageFigure({ h: 1.86 + Math.random() * 0.12, r: 0.5, outer: PROC[i][0], inner: cloth(PROC[i][1]), trim: bronzeMat, eye: new THREE.Color(1.6, 0.9, 0.3).multiplyScalar(0.5), crystal: new THREE.Color(0.9, 1.7, 3.2), staff: i !== 0 });
  const m = { ...f, u: i * 0.05, speed: 0.0105, censer: null, lantern: null, ph: Math.random() * 6 };
  if (i === 0) { const c = censer(); c.pivot.position.copy(f.armR.hand.position); f.armR.p.add(c.pivot); m.censer = c; }
  figuresG.add(f.g); monks.push(m);
}
// I due arcimaghi che governano i pezzi, ai lati della scacchiera
const archmages = {};
[['w', -6.8, Math.PI / 2, cloth(0x8a8274), cloth(0x162036), goldMat, new THREE.Color(0.9, 1.7, 3.2), new THREE.Color(3.2, 2.3, 1.1)],
 ['b', 6.8, -Math.PI / 2, cloth(0x0d0b0f), cloth(0x3c0a12), silverMat, new THREE.Color(3.4, 0.45, 0.15), new THREE.Color(3.4, 0.55, 0.16)]].forEach(([c, x, ry, outer, inner, trim, eye, cry]) => {
  const f = mageFigure({ h: 1.95, r: 0.52, outer, inner, trim, eye, crystal: cry, staff: true, runes: true, light: 0.6 });
  f.g.scale.setScalar(1.32); f.g.position.set(x, FY, 0); f.g.rotation.y = ry; figuresG.add(f.g);
  const N = 46, pos = new Float32Array(N * 3), geo = new THREE.BufferGeometry(); geo.setAttribute('position', new THREE.BufferAttribute(pos, 3));
  const stream = new THREE.Points(geo, new THREE.PointsMaterial({ size: 0.16, map: glowTex, color: new THREE.Color(cry.r / 3.4, cry.g / 3.4, cry.b / 3.4), transparent: true, opacity: 0, blending: THREE.AdditiveBlending, depthWrite: false }));
  stream.frustumCulled = false; stream.renderOrder = 6; scene.add(stream);
  archmages[c] = { f, cast: 0, target: null, ph: Math.random() * 6, stream, N, pos };
});
function castMagic(color, mesh, strength) { const a = archmages[color]; if (!a) return; a.cast = Math.max(a.cast, strength); a.target = mesh; }
const _v1 = new THREE.Vector3(), _v2 = new THREE.Vector3(), _v3 = new THREE.Vector3(), _curve = new THREE.QuadraticBezierCurve3(_v1, _v2, _v3);
function updateArchmages(t, dt) {
  Object.values(archmages).forEach(a => { const f = a.f, c = a.cast; a.cast = Math.max(0, a.cast - dt * 0.42);
    f.armR.p.rotation.x += ((-0.45 - c * 1.35) - f.armR.p.rotation.x) * Math.min(1, dt * 5);
    f.armR.p.rotation.z += ((0.08 + c * 0.35) - f.armR.p.rotation.z) * Math.min(1, dt * 5);
    f.head.rotation.x = 0.14 - c * 0.12 + Math.sin(t * 0.5 + a.ph) * 0.03; f.head.rotation.y = Math.sin(t * 0.23 + a.ph) * 0.12 * (1 - c);
    f.g.scale.y = 1.32 * (1 + Math.sin(t * 1.3 + a.ph) * 0.005);
    const pulse = 0.75 + 0.25 * Math.sin(t * 2.2 + a.ph);
    f.crystalMat.color.copy(f.baseCry).multiplyScalar(pulse * 0.7 + c * 1.1); f.glow.scale.setScalar(0.8 + c * 1.6 + pulse * 0.2); f.top.userData.cry.rotation.y += dt * (1.2 + c * 4);
    if (f.pl) f.pl.intensity = (0.45 + c * 2.2) * pulse; f.eyeMat.color.copy(f.baseEye).multiplyScalar(0.7 + c * 1.4);
    if (f.runeG) { f.runeG.rotation.y += dt * (0.25 + c * 2.5); f.runeG.position.y = f.h * 0.55 + Math.sin(t * 0.8 + a.ph) * 0.06; f.runeG.children.forEach((r, i) => r.material.opacity = 0.1 + c * 0.75 + Math.sin(t * 2 + i) * 0.04); }
    a.stream.material.opacity = Math.min(1, c * 1.4);
    if (c > 0.02 && a.target) { f.top.getWorldPosition(_v1); _v3.copy(a.target.position); _v3.y += 0.8; _v2.copy(_v1).lerp(_v3, 0.5); _v2.y += 1.6;
      for (let i = 0; i < a.N; i++) { const u = (i / a.N + t * 0.9) % 1; const p = _curve.getPoint(u);
        a.pos[i * 3] = p.x + Math.sin(t * 9 + i) * 0.04; a.pos[i * 3 + 1] = p.y + Math.cos(t * 7 + i) * 0.04; a.pos[i * 3 + 2] = p.z; }
      a.stream.geometry.attributes.position.needsUpdate = true; } });
}

// Vescovo all'altare
const bishop = (() => {
  const g = new THREE.Group(); g.position.set(0, SY, -25.5); figuresG.add(g);
  const alb = new THREE.Mesh(makeRobe(1.95, 0.48, 12), swayMat(albMat, 0.5)); alb.castShadow = true; g.add(alb);
  const chas = lathe([[0.62, 0.95], [0.56, 1.2], [0.48, 1.45], [0.32, 1.66], [0.16, 1.76]], 30, chasubleMat); chas.scale.z = 0.62; g.add(chas);
  box(0.12, 0.75, 0.02, goldMat, 0, 1.35, 0.3, g, false); box(0.45, 0.1, 0.02, goldMat, 0, 1.52, 0.25, g, false);
  const neck = new THREE.Mesh(new THREE.CylinderGeometry(0.07, 0.08, 0.12, 10), skinMat); neck.position.y = 1.8; g.add(neck);
  const head = new THREE.Mesh(new THREE.SphereGeometry(0.14, 16, 12), skinMat); head.position.y = 1.95; head.scale.set(0.9, 1.1, 1); g.add(head);
  const beard = new THREE.Mesh(new THREE.ConeGeometry(0.1, 0.25, 10), cloth(0x8a8478)); beard.position.set(0, 1.82, 0.08); beard.rotation.x = Math.PI + 0.3; g.add(beard);
  const mitre = new THREE.Mesh(extrude(new THREE.Shape(archFill(0.15, 0.3, 0, 0.14)), 0.2, 1), new THREE.MeshStandardMaterial({ color: 0xd8d0bc, roughness: 0.6 })); mitre.position.y = 2.02; g.add(mitre);
  box(0.32, 0.05, 0.22, goldMat, 0, 2.06, 0, g, false); box(0.04, 0.4, 0.21, goldMat, 0, 2.25, 0, g, false);
  const arms = [-1, 1].map(s => { const p = new THREE.Group(); p.position.set(s * 0.3, 1.62, 0.05); g.add(p);
    const sl = new THREE.Mesh(new THREE.CylinderGeometry(0.1, 0.15, 0.6, 10), albMat); sl.position.y = -0.28; p.add(sl);
    const hand = new THREE.Mesh(new THREE.SphereGeometry(0.06, 8, 6), skinMat); hand.position.y = -0.6; p.add(hand); return p; });
  const croz = new THREE.Group(); croz.position.set(-0.62, 0, 0.15); g.add(croz);
  const staff = new THREE.Mesh(new THREE.CylinderGeometry(0.02, 0.02, 2.3, 6), goldMat); staff.position.y = 1.15; croz.add(staff);
  const crook = new THREE.Mesh(new THREE.TorusGeometry(0.12, 0.02, 6, 16, Math.PI * 1.4), goldMat); crook.position.set(0.08, 2.35, 0); crook.rotation.z = -0.4; croz.add(crook);
  return { g, arms, head };
})();

// Coro di monaci negli stalli
const choir = [];
[-1, 1].forEach(side => [-15.9, -14.5, -13.1].forEach((z, i) => {
  const f = hoodedFigure({ h: 1.82, r: 0.46, mat: habitBlack });
  const sur = lathe([[0.56, 0.75], [0.5, 1.0], [0.42, 1.3], [0.32, 1.5], [0.18, 1.6]], 26, surpliceMat); f.g.add(sur);
  const book = new THREE.Group(); book.position.set(0, 1.12, 0.52); book.rotation.x = -0.7; f.g.add(book);
  [-1, 1].forEach(s => { const p = new THREE.Mesh(new THREE.BoxGeometry(0.16, 0.22, 0.015), cloth(0xd0c8b0)); p.position.x = s * 0.08; p.rotation.y = -s * 0.25; book.add(p); });
  f.g.position.set(side * 6.15, FY + 0.15, z); f.g.rotation.y = -side * Math.PI / 2;
  figuresG.add(f.g); choir.push({ ...f, phase: i * 1.7 + side });
}));

// Penitenti inginocchiati davanti al presbiterio
const penitents = [];
[-1.6, 0, 1.6].forEach((x, i) => {
  const f = hoodedFigure({ h: 1.4, r: 0.48, mat: i === 1 ? habitBrown : habitBlack });
  f.g.position.set(x, FY - 0.05, -16.5); f.g.rotation.y = Math.PI; f.g.scale.set(1, 0.85, 1.1); figuresG.add(f.g);
  const cg = new THREE.Group(); cg.position.set(x, FY, -17.0); figuresG.add(cg); addCandle(cg, 0, 0, 0, 0.35, 1.1);
  penitents.push({ ...f, phase: i * 1.1 });
});

// Pipistrelli sotto la volta
const bats = [];
const batMat = new THREE.MeshStandardMaterial({ color: 0x0c0a0e, roughness: 0.9, side: THREE.DoubleSide });
const batWing = new THREE.ShapeGeometry(new THREE.Shape([new V2(0, 0.05), new V2(0.35, 0.15), new V2(0.7, 0.1), new V2(0.62, -0.05), new V2(0.5, -0.02), new V2(0.4, -0.12), new V2(0.25, -0.04), new V2(0, -0.1)]));
for (let i = 0; i < 10; i++) {
  const g = new THREE.Group(); const body = new THREE.Mesh(new THREE.SphereGeometry(0.09, 8, 6), batMat); body.scale.set(1, 0.8, 1.8); g.add(body);
  const lp = new THREE.Group(), rp = new THREE.Group(); const lw = new THREE.Mesh(batWing, batMat); lw.rotation.x = -Math.PI / 2; lp.add(lw);
  const rw = new THREE.Mesh(batWing, batMat); rw.rotation.x = -Math.PI / 2; rw.rotation.z = Math.PI; rw.rotation.x = Math.PI / 2; rp.add(rw);
  g.add(lp, rp); creaturesG.add(g);
  bats.push({ g, lp, rp, a: Math.random() * 6.28, rx: 3 + Math.random() * 4.5, rz: 10 + Math.random() * 14, sp: 0.004 + Math.random() * 0.006, y: FY + 25 + Math.random() * 8, fl: 9 + Math.random() * 6, ph: Math.random() * 6 });
}
// Corvi appollaiati
const crows = [];
function crow(x, y, z, ry) {
  const g = new THREE.Group(); const m = new THREE.MeshStandardMaterial({ color: 0x08080b, roughness: 0.45, metalness: 0.35 });
  const b = new THREE.Mesh(new THREE.SphereGeometry(0.16, 12, 10), m); b.scale.set(0.8, 0.9, 1.4); g.add(b);
  const h = new THREE.Group(); h.position.set(0, 0.16, 0.16); g.add(h);
  h.add(new THREE.Mesh(new THREE.SphereGeometry(0.09, 10, 8), m));
  const beak = new THREE.Mesh(new THREE.ConeGeometry(0.03, 0.14, 6), m); beak.rotation.x = Math.PI / 2; beak.position.z = 0.12; h.add(beak);
  [-1, 1].forEach(s => { const e = makeGlow(0xffb030, 0.06, 1); e.position.set(s * 0.045, 0.02, 0.06); h.add(e); });
  const tail = new THREE.Mesh(new THREE.BoxGeometry(0.1, 0.02, 0.26), m); tail.position.set(0, -0.04, -0.28); tail.rotation.x = 0.35; g.add(tail);
  g.position.set(x, y, z); g.rotation.y = ry; creaturesG.add(g); crows.push({ h, ph: Math.random() * 10 });
}
crow(-3.8, SY + 7.4, -27.0, 0.3); crow(2.9, SY + 7.4, -27.0, -0.4); crow(-13, FY + 1.45, -9.2, 2.5);

// =====================================================================
// OGGETTI E ANIMAZIONI AGGIUNTIVE
// =====================================================================
// --- Clessidra che si capovolge a ogni turno ---
const hourglass = (() => {
  const root = new THREE.Group(); root.position.set(-5.05, PL, 0.0); root.scale.setScalar(1.5); scene.add(root);
  const ped = new THREE.Mesh(new THREE.CylinderGeometry(0.22, 0.3, 0.28, 12), darkStoneMat); ped.position.y = 0.14; ped.castShadow = true; root.add(ped);
  const frame = new THREE.Group(); frame.position.y = 0.28 + 0.42; root.add(frame);
  [-0.4, 0.4].forEach(y => { const cap = new THREE.Mesh(new THREE.CylinderGeometry(0.2, 0.2, 0.05, 16), woodMat); cap.position.y = y; cap.castShadow = true; frame.add(cap); });
  for (let i = 0; i < 3; i++) { const a = i / 3 * Math.PI * 2; const post = new THREE.Mesh(new THREE.CylinderGeometry(0.012, 0.012, 0.8, 6), goldMat); post.position.set(Math.cos(a) * 0.17, 0, Math.sin(a) * 0.17); frame.add(post); }
  const glassMat = new THREE.MeshStandardMaterial({ color: 0xcfd8e0, transparent: true, opacity: 0.18, roughness: 0.05, metalness: 0.1, depthWrite: false });
  const bulb = new THREE.LatheGeometry([[0.01, -0.38], [0.12, -0.34], [0.14, -0.24], [0.1, -0.1], [0.02, 0], [0.1, 0.1], [0.14, 0.24], [0.12, 0.34], [0.01, 0.38]].map(p => new V2(p[0], p[1])), 20);
  const glass = new THREE.Mesh(bulb, glassMat); glass.renderOrder = 4; frame.add(glass);
  const sandMat = new THREE.MeshStandardMaterial({ color: 0xc9a46a, roughness: 0.9, emissive: 0x2a1a08 });
  const sandG = new THREE.Group(); sandG.position.y = 0.7; root.add(sandG);
  const topGeo = new THREE.ConeGeometry(0.115, 0.24, 16); topGeo.rotateX(Math.PI); topGeo.translate(0, 0.12, 0);
  const topSand = new THREE.Mesh(topGeo, sandMat); topSand.position.y = 0.02; sandG.add(topSand);
  const botGeo = new THREE.ConeGeometry(0.12, 0.14, 16); botGeo.translate(0, 0.07, 0);
  const botSand = new THREE.Mesh(botGeo, sandMat); botSand.position.y = -0.36; sandG.add(botSand);
  const stream = new THREE.Mesh(new THREE.CylinderGeometry(0.004, 0.004, 0.36, 4), sandMat); stream.position.y = -0.17; sandG.add(stream);
  const glow = makeGlow(0xffc070, 0.6, 0.18); glow.position.y = 0.7; root.add(glow);
  return { root, frame, sandG, topSand, botSand, stream, flip: 0, turnStart: 0, lastTurn: 'w' };
})();
function updateHourglass(t, dt) {
  const hg = hourglass;
  if (game.turn() !== hg.lastTurn) { hg.lastTurn = game.turn(); hg.flip = 1; hg.turnStart = t; }
  if (hg.flip > 0) { hg.flip = Math.max(0, hg.flip - dt / 0.8); const e = 1 - hg.flip; hg.frame.rotation.z = (1 - Math.pow(1 - e, 3)) * Math.PI; hg.sandG.visible = false; if (hg.flip === 0) { hg.frame.rotation.z = 0; hg.sandG.visible = true; } }
  let p;
  if (timerMode !== 'off') { const total = timerMode === '3m' ? 180 : 300, left = game.turn() === 'w' ? timeWhite : timeBlack; p = 1 - left / total; }
  else p = Math.min(1, (t - hg.turnStart) / 60);
  p = THREE.MathUtils.clamp(p, 0, 1);
  const st = Math.cbrt(Math.max(1 - p, 0.0001)), sb = Math.cbrt(Math.max(p, 0.0001));
  hg.topSand.scale.setScalar(st); hg.botSand.scale.setScalar(sb); hg.stream.visible = p > 0.005 && p < 0.995 && !game.game_over();
}

// --- Cantoria con organo a canne sopra il portale (le canne si illuminano con la musica) ---
const organPipes = [];
(() => {
  const g = new THREE.Group(); g.position.set(0, FY + 10, LEN - 1.6); staticG.add(g);
  box(17.6, 0.5, 2.8, stoneMat, 0, 0, 0.2, g);
  for (let i = 0; i < 16; i++) { const x = -7.9 + i * 1.053; const a = new THREE.Mesh(archRing(0.3, 0.6, 0.72, 0.5, 0.15), stoneMat); a.position.set(x, 0.25, -1.15); g.add(a); }
  box(17.6, 0.18, 0.3, stoneMat, 0, 1.55, -1.15, g); box(17.6, 0.2, 0.3, stoneMat, 0, 0.3, -1.15, g);
  [-6.5, 0, 6.5].forEach(x => { const c = lathe([[0.001, 0], [0.25, 0], [0.7, 0.9], [0.001, 0.9]], 8, stoneMat); c.rotation.x = Math.PI; c.position.set(x, -0.25, -0.9); g.add(c); });
  box(14, 7.5, 1.2, woodMat, 0, 4.0, 1.0, g);
  box(14.4, 0.4, 1.5, woodMat, 0, 7.9, 1.0, g);
  for (let k = 0; k < 7; k++) { const sp = new THREE.Mesh(new THREE.ConeGeometry(0.22, 1.4, 4), woodMat); sp.position.set(-6.6 + k * 2.2, 8.8, 1.0); g.add(sp); }
  const groups = [new THREE.MeshStandardMaterial({ color: 0xb8945a, roughness: 0.35, metalness: 0.6, emissive: 0xff9a40, emissiveIntensity: 0 }),
                  new THREE.MeshStandardMaterial({ color: 0xb8945a, roughness: 0.35, metalness: 0.6, emissive: 0xff9a40, emissiveIntensity: 0 }),
                  new THREE.MeshStandardMaterial({ color: 0xb8945a, roughness: 0.35, metalness: 0.6, emissive: 0xff9a40, emissiveIntensity: 0 })];
  const pg = new THREE.Group(); pg.position.copy(g.position); scene.add(pg);
  for (let i = 0; i < 27; i++) { const d = Math.abs(i - 13), h = 2.2 + (13 - d) * 0.34 + (i % 2) * 0.35, x = -6.5 + i * 0.5, mat = groups[i % 3];
    const pipe = new THREE.Mesh(new THREE.CylinderGeometry(0.15, 0.15, h, 12), mat); pipe.position.set(x, 1.2 + h / 2, 0.25); pg.add(pipe);
    const mouth = new THREE.Mesh(new THREE.BoxGeometry(0.16, 0.12, 0.05), new THREE.MeshBasicMaterial({ color: 0x050403 })); mouth.position.set(x, 1.6, 0.4); pg.add(mouth);
    const foot = new THREE.Mesh(new THREE.ConeGeometry(0.15, 0.35, 12), mat); foot.rotation.x = Math.PI; foot.position.set(x, 1.05, 0.25); pg.add(foot); }
  groups.forEach((m, i) => organPipes.push({ m, ph: i * 2.1 }));
})();

// --- Lanterne pendenti nelle navate laterali, che oscillano ---
const swingLanterns = [];
[-1, 1].forEach(side => bayZ.forEach((z, i) => {
  if (i % 2) return;
  const pivot = new THREE.Group(); pivot.position.set(side * 13, FY + 13, z); figuresG.add(pivot);
  const chain = new THREE.Mesh(new THREE.CylinderGeometry(0.012, 0.012, 4.6, 4), ironMat); chain.position.y = -2.3; pivot.add(chain);
  const l = lantern(); l.scale.setScalar(2.2); l.position.y = -4.9; pivot.add(l);
  swingLanterns.push({ pivot, ph: Math.random() * 6, amp: 0.05 + Math.random() * 0.05 });
}));

// --- Nebbia al suolo (strati che scorrono lentamente) ---
const mistTex = (() => { const c = mkCanvas(256), g = c.getContext('2d');
  for (let i = 0; i < 70; i++) { const x = Math.random() * 256, y = Math.random() * 256, r = 20 + Math.random() * 60;
    [[0, 0], [256, 0], [-256, 0], [0, 256], [0, -256]].forEach(([ox, oy]) => { const gr = g.createRadialGradient(x + ox, y + oy, 0, x + ox, y + oy, r); gr.addColorStop(0, 'rgba(255,255,255,0.22)'); gr.addColorStop(1, 'rgba(255,255,255,0)'); g.fillStyle = gr; g.fillRect(0, 0, 256, 256); }); }
  const t = new THREE.CanvasTexture(c); t.wrapS = t.wrapT = THREE.RepeatWrapping; return t; })();
const mistG = new THREE.Group(); scene.add(mistG); const mistLayers = [];
[[FY + 0.1, 0.16], [FY + 0.32, 0.11], [FY + 0.55, 0.07]].forEach(([y, op], i) => {
  const tex = mistTex.clone(); tex.needsUpdate = true; tex.repeat.set(3 + i, 5 + i);
  const m = new THREE.Mesh(new THREE.PlaneGeometry(33, 55), new THREE.MeshBasicMaterial({ map: tex, color: 0x505870, transparent: true, opacity: op, depthWrite: false, fog: true }));
  m.rotation.x = -Math.PI / 2; m.position.y = y; m.renderOrder = 3; mistG.add(m); mistLayers.push({ tex, sp: 0.004 + i * 0.003, dir: i % 2 ? 1 : -1 });
});

// --- Chiazze di luce colorata delle vetrate sul pavimento ---
const patchG = new THREE.Group(); scene.add(patchG); const patches = [];
bayZ.forEach((z, i) => {
  const mat = new THREE.MeshBasicMaterial({ map: glassMats[i % glassMats.length].map, transparent: true, opacity: 0.12, blending: THREE.AdditiveBlending, depthWrite: false, polygonOffset: true, polygonOffsetFactor: -1, polygonOffsetUnits: -1 });
  const m = new THREE.Mesh(new THREE.PlaneGeometry(2.6, 5.2), mat); m.rotation.x = -Math.PI / 2; m.rotation.z = Math.PI / 2 + 0.25; m.position.set(-2.2, FY + 0.02, z + 1.2);
  if (Math.abs(z) < 6) return;                                        // non sopra la pedana della scacchiera
  patchG.add(m); patches.push({ m, ph: Math.random() * 6 });
});

// --- Topi che corrono lungo i muri delle navate ---
const rats = [];
for (let i = 0; i < 3; i++) {
  const g = new THREE.Group(); const m = new THREE.MeshStandardMaterial({ color: 0x2a2420, roughness: 0.9 });
  const b = new THREE.Mesh(new THREE.SphereGeometry(0.09, 10, 8), m); b.scale.set(0.9, 0.75, 1.8); b.position.y = 0.07; g.add(b);
  const h = new THREE.Mesh(new THREE.ConeGeometry(0.06, 0.14, 8), m); h.rotation.x = Math.PI / 2; h.position.set(0, 0.07, 0.2); g.add(h);
  [-1, 1].forEach(s => { const e = new THREE.Mesh(new THREE.SphereGeometry(0.025, 6, 4), m); e.position.set(s * 0.04, 0.12, 0.14); g.add(e); });
  const tail = new THREE.Mesh(new THREE.CylinderGeometry(0.006, 0.01, 0.3, 4), m); tail.rotation.x = Math.PI / 2 - 0.2; tail.position.set(0, 0.05, -0.3); g.add(tail);
  creaturesG.add(g); rats.push({ g, side: i % 2 ? 1 : -1, z: -20 + i * 15, dir: 1, speed: 0, wait: Math.random() * 6, tail });
}
function updateRats(t, dt) {
  rats.forEach(r => {
    if (r.wait > 0) { r.wait -= dt; r.speed = 0; if (r.wait <= 0) { r.dir = Math.random() > 0.5 ? 1 : -1; r.target = THREE.MathUtils.clamp(r.z + r.dir * (4 + Math.random() * 12), -25, 25); } }
    else { r.speed = 3.2; r.z += r.dir * r.speed * dt; if ((r.dir > 0 && r.z >= r.target) || (r.dir < 0 && r.z <= r.target)) r.wait = 2 + Math.random() * 7; }
    r.g.position.set(r.side * 16.55, FY, r.z); r.g.rotation.y = r.dir > 0 ? 0 : Math.PI;
    r.g.position.y = FY + (r.speed ? Math.abs(Math.sin(t * 30)) * 0.02 : 0); r.tail.rotation.z = Math.sin(t * (r.speed ? 20 : 3)) * 0.3;
  });
}

// --- Temporale: lampi che illuminano le vetrate, e tuono ---
let nextFlash = 8 + Math.random() * 10, flashT = -1;
function flashCurve(t) { return Math.max(Math.exp(-Math.pow((t - 0.04) / 0.04, 2)), 0.6 * Math.exp(-Math.pow((t - 0.2) / 0.05, 2)), 0.85 * Math.exp(-Math.pow((t - 0.38) / 0.06, 2))); }

// =====================================================================
// SCACCHIERA E PEZZI
// =====================================================================
// =====================================================================
// PEZZI GOTICI (geometria procedurale ad alta definizione, fusa per materiale)
// =====================================================================
const PSEG = IS_MOBILE ? 36 : 56;
function role(r, m) { m.userData.role = r; return m; }
function lat(prof, r = 'body', seg = PSEG) { return role(r, new THREE.Mesh(new THREE.LatheGeometry(prof.map(p => new V2(Math.max(p[0], 0.0005), p[1])), seg))); }
function band(rad, tube, y, r = 'accent') { const m = role(r, new THREE.Mesh(new THREE.TorusGeometry(rad, tube, 8, PSEG))); m.rotation.x = Math.PI / 2; m.position.y = y; return m; }
function beads(g, rad, y, n, size, r = 'accent') { for (let i = 0; i < n; i++) { const a = i / n * Math.PI * 2; const b = role(r, new THREE.Mesh(new THREE.SphereGeometry(size, 8, 6))); b.position.set(Math.cos(a) * rad, y, Math.sin(a) * rad); g.add(b); } }
function at(m, x, y, z, rx = 0, ry = 0, rz = 0) { m.position.set(x, y, z); m.rotation.set(rx, ry, rz); return m; }
// finestra a sesto acuto incassata (si illumina), orientata verso l'esterno a raggio rad e angolo a
function lancet(g, surf, a, y, w, h, tilt = 0, r = 'inset') {
  const legH = Math.max(h - w * 0.87, 0.001);
  const geo = new THREE.ShapeGeometry(new THREE.Shape(archFill(w / 2, w, 0, legH)), 6);
  const m = role(r, new THREE.Mesh(geo)); m.rotation.order = 'YXZ'; m.rotation.set(-tilt, a, 0); m.position.set(Math.sin(a) * (surf + 0.004), y, Math.cos(a) * (surf + 0.004)); g.add(m);
  const fr = role('accent', new THREE.Mesh(new THREE.ShapeGeometry(new THREE.Shape(archFill(w / 2 + 0.009, w + 0.018, -0.009, legH + 0.009)), 6)));
  fr.rotation.order = 'YXZ'; fr.rotation.set(-tilt, a, 0); fr.position.set(Math.sin(a) * (surf + 0.002), y, Math.cos(a) * (surf + 0.002)); g.add(fr);
}
function gothicBase(r) {
  const g = new THREE.Group();
  g.add(lat([[0, 0], [r, 0], [r * 1.015, 0.012], [r * 1.0, 0.04], [r * 0.95, 0.055], [r * 0.95, 0.075], [r * 0.985, 0.088], [r * 0.97, 0.105], [r * 0.86, 0.125], [r * 0.8, 0.15], [r * 0.82, 0.165], [r * 0.74, 0.185], [0, 0.19]]));
  g.add(band(r * 0.955, 0.009, 0.065));
  beads(g, r * 0.83, 0.158, 22, 0.011);
  g.add(lat([[0, -0.001], [r * 0.98, -0.001], [r * 0.98, 0.004], [0, 0.004]], 'dark'));
  return g;
}
function crockets(g, rad, y0, y1, n, size, twist = 0) { // foglie rampanti dei pinnacoli
  for (let i = 0; i < n; i++) { const t = i / (n - 1), a = i * 2.4 + twist, r = rad * (1 - t * 0.85), y = y0 + (y1 - y0) * t;
    const c = role('body', new THREE.Mesh(new THREE.ConeGeometry(size, size * 2.6, 5))); c.position.set(Math.cos(a) * r, y, Math.sin(a) * r);
    c.lookAt(Math.cos(a) * r * 3, y + size * 2, Math.sin(a) * r * 3); c.rotateX(Math.PI / 2); g.add(c); }
}
function pinnacle(g, x, z, y, h, r) {
  g.add(at(role('body', new THREE.Mesh(new THREE.CylinderGeometry(r, r * 1.1, h * 0.45, 8))), x, y + h * 0.225, z));
  g.add(at(role('accent', new THREE.Mesh(new THREE.TorusGeometry(r * 1.05, r * 0.18, 6, 12))), x, y + h * 0.45, z, Math.PI / 2));
  g.add(at(role('body', new THREE.Mesh(new THREE.ConeGeometry(r * 1.05, h * 0.55, 8))), x, y + h * 0.725, z));
  g.add(at(role('accent', new THREE.Mesh(new THREE.SphereGeometry(r * 0.45, 8, 6))), x, y + h + r * 0.3, z));
}
function fleuryCross(g, y, s) {
  g.add(at(role('accent', new THREE.Mesh(new THREE.BoxGeometry(0.03 * s, 0.24 * s, 0.03 * s))), 0, y + 0.12 * s, 0));
  g.add(at(role('accent', new THREE.Mesh(new THREE.BoxGeometry(0.16 * s, 0.03 * s, 0.03 * s))), 0, y + 0.16 * s, 0));
  [[0, y + 0.255 * s], [-0.085 * s, y + 0.16 * s], [0.085 * s, y + 0.16 * s]].forEach(([x, yy]) => {
    const f = role('accent', new THREE.Mesh(new THREE.OctahedronGeometry(0.024 * s))); f.position.set(x, yy, 0); f.scale.set(1, 1.3, 1); g.add(f); });
  g.add(at(role('accent', new THREE.Mesh(new THREE.SphereGeometry(0.03 * s, 12, 8))), 0, y, 0));
}
function makePawn() {
  const g = gothicBase(0.32);
  g.add(lat([[0, 0.19], [0.24, 0.19], [0.215, 0.215], [0.17, 0.27], [0.135, 0.36], [0.115, 0.46], [0.112, 0.5], [0.155, 0.515], [0.17, 0.535], [0.162, 0.552], [0.11, 0.566], [0.098, 0.6], [0, 0.6]]));
  g.add(band(0.165, 0.011, 0.535));
  g.add(lat([[0, 0.585], [0.1, 0.59], [0.138, 0.625], [0.157, 0.68], [0.156, 0.74], [0.138, 0.8], [0.108, 0.855], [0.072, 0.905], [0.042, 0.95], [0.02, 0.99], [0, 1.015]]));
  lancet(g, 0.157, 0, 0.655, 0.055, 0.085, 0.05);                        // apertura del cappuccio: la luce interna
  crockets(g, 0.12, 0.8, 0.97, 5, 0.012);
  g.add(at(role('accent', new THREE.Mesh(new THREE.SphereGeometry(0.022, 12, 8))), 0, 1.028, 0));
  return g;
}
function makeRook() {
  const g = gothicBase(0.36);
  g.add(lat([[0, 0.19], [0.3, 0.19], [0.3, 0.205], [0.272, 0.235], [0.258, 0.3], [0.248, 0.45], [0.244, 0.62], [0.246, 0.78], [0.258, 0.82], [0.29, 0.86], [0.325, 0.9], [0.335, 0.93], [0.335, 1.0], [0.3, 1.0], [0.3, 0.975], [0, 0.975]]));
  g.add(band(0.252, 0.01, 0.3)); g.add(band(0.262, 0.011, 0.815)); g.add(band(0.338, 0.008, 0.93));
  for (let i = 0; i < 4; i++) { const a = i * Math.PI / 2; lancet(g, 0.249, a, 0.46, 0.075, 0.24, 0.02);
    const ro = role('inset', new THREE.Mesh(new THREE.CircleGeometry(0.026, 20))); ro.position.set(Math.sin(a) * 0.25, 0.74, Math.cos(a) * 0.25); ro.rotation.y = a; g.add(ro);
    const rr = role('accent', new THREE.Mesh(new THREE.TorusGeometry(0.03, 0.005, 6, 20))); rr.position.copy(ro.position); rr.rotation.y = a; g.add(rr); }
  for (let i = 0; i < 8; i++) { const a = i * Math.PI / 4 + Math.PI / 8;                      // contrafforti
    const b = role('body', new THREE.Mesh(new THREE.BoxGeometry(0.035, 0.42, 0.05))); b.position.set(Math.sin(a) * 0.255, 0.5, Math.cos(a) * 0.255); b.rotation.y = a; g.add(b);
    const c = role('body', new THREE.Mesh(new THREE.ConeGeometry(0.03, 0.08, 4))); c.position.set(Math.sin(a) * 0.255, 0.75, Math.cos(a) * 0.255); c.rotation.y = a + Math.PI / 4; g.add(c); }
  for (let i = 0; i < 12; i++) { const a = i / 12 * Math.PI * 2;                                  // merli a punta
    const m = role('body', new THREE.Mesh(new THREE.BoxGeometry(0.075, 0.075, 0.045))); m.position.set(Math.sin(a) * 0.315, 1.035, Math.cos(a) * 0.315); m.rotation.y = a; g.add(m);
    const p = role('body', new THREE.Mesh(new THREE.ConeGeometry(0.052, 0.07, 4))); p.position.set(Math.sin(a) * 0.315, 1.105, Math.cos(a) * 0.315); p.rotation.y = a + Math.PI / 4; p.scale.z = 0.62; g.add(p); }
  [Math.PI / 4, 3 * Math.PI / 4, 5 * Math.PI / 4, 7 * Math.PI / 4].forEach(a => pinnacle(g, Math.sin(a) * 0.335, Math.cos(a) * 0.335, 0.88, 0.36, 0.045));
  g.add(lat([[0, 0.975], [0.12, 0.975], [0.11, 1.05], [0.075, 1.12], [0, 1.16]]));                  // torretta centrale
  g.add(at(role('accent', new THREE.Mesh(new THREE.SphereGeometry(0.022, 10, 8))), 0, 1.175, 0));
  return g;
}
function makeBishop() {
  const g = gothicBase(0.35);
  g.add(lat([[0, 0.19], [0.27, 0.19], [0.25, 0.215], [0.195, 0.27], [0.15, 0.38], [0.122, 0.52], [0.108, 0.66], [0.106, 0.7], [0.152, 0.715], [0.168, 0.735], [0.155, 0.755], [0.105, 0.77], [0.097, 0.8], [0, 0.8]]));
  g.add(band(0.165, 0.011, 0.735)); g.add(band(0.196, 0.008, 0.27));
  g.add(lat([[0, 0.785], [0.1, 0.79], [0.142, 0.835], [0.168, 0.9], [0.178, 0.98], [0.172, 1.06], [0.152, 1.14], [0.122, 1.21], [0.085, 1.28], [0.048, 1.34], [0.02, 1.385], [0, 1.4]]));
  g.add(band(0.135, 0.01, 0.82));
  lancet(g, 0.178, 0, 1.0, 0.05, 0.13, 0.12); lancet(g, 0.178, Math.PI, 1.0, 0.05, 0.13, 0.12);
  for (let i = 0; i < 6; i++) lancet(g, 0.133, i * Math.PI / 3 + Math.PI / 6, 0.46, 0.04, 0.12, 0.14);
  crockets(g, 0.14, 1.12, 1.36, 6, 0.012, 0.6);
  fleuryCross(g, 1.405, 0.85);
  return g;
}
function makeQueen() {
  const g = gothicBase(0.38);
  g.add(lat([[0, 0.19], [0.3, 0.19], [0.28, 0.215], [0.22, 0.28], [0.17, 0.4], [0.138, 0.56], [0.122, 0.72], [0.115, 0.88], [0.113, 0.96], [0.162, 0.98], [0.18, 1.0], [0.165, 1.022], [0.118, 1.036], [0.112, 1.08], [0.132, 1.12], [0.17, 1.18], [0.205, 1.245], [0.215, 1.28], [0.19, 1.28], [0, 1.28]]));
  g.add(band(0.176, 0.012, 1.0)); g.add(band(0.222, 0.009, 0.28)); g.add(band(0.212, 0.01, 1.27));
  for (let i = 0; i < 6; i++) lancet(g, 0.141, i * Math.PI / 3, 0.55, 0.045, 0.2, 0.1);
  for (let i = 0; i < 8; i++) { const a = i / 8 * Math.PI * 2; pinnacle(g, Math.sin(a) * 0.19, Math.cos(a) * 0.19, 1.27, 0.24, 0.022);
    const gab = role('body', new THREE.Mesh(extrude(new THREE.Shape(archFill(0.045, 0.09, 0, 0.01)), 0.018, 1))); const a2 = a + Math.PI / 8;
    gab.position.set(Math.sin(a2) * 0.2, 1.275, Math.cos(a2) * 0.2); gab.rotation.y = a2; g.add(gab); }
  g.add(lat([[0, 1.275], [0.15, 1.275], [0.142, 1.33], [0.118, 1.4], [0.08, 1.46], [0.04, 1.5], [0, 1.52]]));
  g.add(band(0.149, 0.008, 1.29));
  g.add(at(role('accent', new THREE.Mesh(new THREE.SphereGeometry(0.048, 20, 14))), 0, 1.56, 0));
  g.add(at(role('accent', new THREE.Mesh(new THREE.TorusGeometry(0.049, 0.006, 6, 24))), 0, 1.56, 0, Math.PI / 2));
  g.add(at(role('accent', new THREE.Mesh(new THREE.ConeGeometry(0.012, 0.09, 6))), 0, 1.645, 0));
  return g;
}
function makeKing() {
  const g = gothicBase(0.4);
  g.add(lat([[0, 0.19], [0.32, 0.19], [0.3, 0.215], [0.24, 0.285], [0.185, 0.42], [0.152, 0.6], [0.138, 0.78], [0.13, 0.95], [0.128, 1.04], [0.18, 1.06], [0.198, 1.085], [0.182, 1.108], [0.13, 1.122], [0.122, 1.17], [0.145, 1.22], [0.19, 1.3], [0.235, 1.38], [0.245, 1.42], [0.215, 1.42], [0, 1.42]]));
  g.add(band(0.196, 0.013, 1.085)); g.add(band(0.242, 0.01, 0.285)); g.add(band(0.244, 0.011, 1.41));
  for (let i = 0; i < 8; i++) lancet(g, 0.153, i * Math.PI / 4, 0.6, 0.042, 0.24, 0.07);
  for (let i = 0; i < 4; i++) { const a = i * Math.PI / 2;                                         // quattro gabbie gotiche della corona
    const gab = role('body', new THREE.Mesh(extrude(new THREE.Shape(archFill(0.085, 0.17, 0, 0.04)), 0.028, 1)));
    gab.position.set(Math.sin(a) * 0.205, 1.415, Math.cos(a) * 0.205); gab.rotation.y = a; g.add(gab);
    const win = role('inset', new THREE.Mesh(new THREE.ShapeGeometry(new THREE.Shape(archFill(0.04, 0.08, 0, 0.03)), 6)));
    win.position.set(Math.sin(a) * 0.221, 1.44, Math.cos(a) * 0.221); win.rotation.y = a; g.add(win);
    pinnacle(g, Math.sin(a + Math.PI / 4) * 0.225, Math.cos(a + Math.PI / 4) * 0.225, 1.41, 0.3, 0.026); }
  g.add(lat([[0, 1.415], [0.165, 1.415], [0.156, 1.48], [0.128, 1.56], [0.085, 1.63], [0.04, 1.68], [0, 1.7]]));
  g.add(band(0.163, 0.009, 1.43));
  fleuryCross(g, 1.7, 1.25);
  return g;
}
function makeKnight() {
  const g = gothicBase(0.36);
  g.add(lat([[0, 0.19], [0.27, 0.19], [0.25, 0.215], [0.22, 0.25], [0.215, 0.27], [0, 0.27]]));
  g.add(band(0.222, 0.009, 0.25));
  const sh = new THREE.Shape(); sh.moveTo(-0.17, 0.25);
  sh.splineThru([[-0.19, 0.44], [-0.175, 0.62], [-0.125, 0.82], [-0.055, 0.97], [0.015, 1.06], [0.075, 1.05], [0.14, 1.0], [0.225, 0.92], [0.3, 0.845], [0.338, 0.78], [0.33, 0.73], [0.285, 0.705], [0.2, 0.72], [0.115, 0.745], [0.07, 0.69], [0.085, 0.57], [0.14, 0.43], [0.19, 0.3], [0.195, 0.25]].map(p => new V2(p[0], p[1])));
  sh.lineTo(-0.17, 0.25);
  const hg = new THREE.ExtrudeGeometry(sh, { depth: 0.14, bevelEnabled: true, bevelThickness: 0.055, bevelSize: 0.032, bevelSegments: 5, curveSegments: 64, steps: 1 });
  hg.translate(0, 0, -0.07); hg.computeVertexNormals();
  const head = role('body', new THREE.Mesh(hg)); const h = new THREE.Group(); h.add(head); g.add(h);
  [-1, 1].forEach(s => {
    h.add(at(role('body', new THREE.Mesh(new THREE.ConeGeometry(0.026, 0.12, 8))), 0.0, 1.11, s * 0.05, s * 0.15, 0, -0.35));
    h.add(at(role('inset', new THREE.Mesh(new THREE.SphereGeometry(0.021, 12, 8))), 0.135, 0.955, s * 0.113));
    h.add(at(role('accent', new THREE.Mesh(new THREE.TorusGeometry(0.026, 0.005, 6, 16))), 0.135, 0.955, s * 0.118));
    h.add(at(role('dark', new THREE.Mesh(new THREE.SphereGeometry(0.016, 8, 6))), 0.318, 0.79, s * 0.058));
  });
  for (let i = 0; i < 9; i++) { const t = i / 8, x = -0.185 + t * 0.18 + Math.sin(t * Math.PI) * -0.02, y = 0.42 + t * 0.62;          // criniera a lame
    const sp = role('accent', new THREE.Mesh(new THREE.ConeGeometry(0.028, 0.13 - t * 0.04, 4))); sp.position.set(x - 0.012, y, 0); sp.rotation.z = 1.25 - t * 0.85; sp.scale.z = 0.4; h.add(sp); }
  const rid = role('accent', new THREE.Mesh(new THREE.BoxGeometry(0.3, 0.018, 0.085))); rid.position.set(0.19, 0.95, 0); rid.rotation.z = -0.72; h.add(rid);   // testiera (chanfrein)
  const spike = role('accent', new THREE.Mesh(new THREE.ConeGeometry(0.018, 0.09, 6))); spike.position.set(0.06, 1.07, 0); spike.rotation.z = -0.5; h.add(spike);
  const bridle = role('accent', new THREE.Mesh(new THREE.TorusGeometry(0.075, 0.009, 6, 24))); bridle.position.set(0.272, 0.765, 0); bridle.rotation.y = Math.PI / 2; bridle.scale.set(1, 1.15, 1); h.add(bridle);
  const collar = role('accent', new THREE.Mesh(new THREE.TorusGeometry(0.17, 0.012, 6, 32))); collar.position.set(0.0, 0.42, 0); collar.rotation.x = Math.PI / 2; collar.scale.set(1.05, 0.62, 1); h.add(collar);
  h.rotation.y = Math.PI;                           // di profilo, come nei set da torneo
  return g;
}
function mergeByRole(group) {
  group.updateMatrixWorld(true); const by = {};
  group.traverse(o => { if (!o.isMesh) return; const r = o.userData.role || 'body';
    let gm = o.geometry.index ? o.geometry.toNonIndexed() : o.geometry.clone(); gm.applyMatrix4(o.matrixWorld);
    if (!gm.attributes.normal) gm.computeVertexNormals(); (by[r] = by[r] || []).push(gm); });
  const out = {};
  Object.entries(by).forEach(([r, list]) => { let n = 0; list.forEach(gm => n += gm.attributes.position.count);
    const P = new Float32Array(n * 3), N = new Float32Array(n * 3); let off = 0;
    list.forEach(gm => { P.set(gm.attributes.position.array, off * 3); N.set(gm.attributes.normal.array, off * 3); off += gm.attributes.position.count; gm.dispose(); });
    const geo = new THREE.BufferGeometry(); geo.setAttribute('position', new THREE.BufferAttribute(P, 3)); geo.setAttribute('normal', new THREE.BufferAttribute(N, 3)); geo.computeBoundingSphere(); out[r] = geo; });
  return out;
}
const pieceGeometries = { p: mergeByRole(makePawn()), r: mergeByRole(makeRook()), n: mergeByRole(makeKnight()), b: mergeByRole(makeBishop()), q: mergeByRole(makeQueen()), k: mergeByRole(makeKing()) };
const PM = (o) => new THREE.MeshPhysicalMaterial(o);
const pieceMats = {
  w: { body: PM({ color: 0xece3d1, roughness: 0.34, metalness: 0.0, clearcoat: 0.55, clearcoatRoughness: 0.22, emissive: 0x17130d }),
       accent: new THREE.MeshStandardMaterial({ color: 0xd6ad62, roughness: 0.28, metalness: 0.6, emissive: 0x2a1906 }),
       inset: new THREE.MeshBasicMaterial({ color: new THREE.Color(3.0, 1.35, 0.38), side: THREE.DoubleSide }),
       dark: new THREE.MeshStandardMaterial({ color: 0x1a1410, roughness: 0.6 }) },
  b: { body: PM({ color: 0x0b0a0d, roughness: 0.2, metalness: 0.12, clearcoat: 1.0, clearcoatRoughness: 0.07, emissive: 0x050407 }),
       accent: new THREE.MeshStandardMaterial({ color: 0xa9adb8, roughness: 0.26, metalness: 0.65, emissive: 0x0d0e12 }),
       inset: new THREE.MeshBasicMaterial({ color: new THREE.Color(5.0, 0.7, 0.18), side: THREE.DoubleSide }),
       dark: new THREE.MeshStandardMaterial({ color: 0x020203, roughness: 0.5 }) } };
const pieceMaterials = { w: pieceMats.w.body, b: pieceMats.b.body };
function applyPieceSet() {}

const boardGroup = new THREE.Group(); scene.add(boardGroup);
const whiteTileMat = new THREE.MeshPhysicalMaterial({ map: marbleW, roughness: 0.3, metalness: 0.0, clearcoat: 0.7, clearcoatRoughness: 0.12, emissive: 0x100e0b });
const blackTileMat = new THREE.MeshPhysicalMaterial({ map: marbleB, roughness: 0.22, metalness: 0.0, clearcoat: 0.9, clearcoatRoughness: 0.08 });
function applyBoardStyle() {}
const frameMat = new THREE.MeshStandardMaterial({ map: woodSet.map, color: 0x6a4a34, roughness: 0.4, metalness: 0.2 });
const pedestal = new THREE.Mesh(new THREE.BoxGeometry(9.2, 0.5, 9.2), frameMat); pedestal.position.y = -0.3; pedestal.receiveShadow = true; pedestal.castShadow = true; boardGroup.add(pedestal);
const frameM = new THREE.Mesh(new THREE.BoxGeometry(9.0, 0.1, 9.0), frameMat); frameM.position.y = 0.0; frameM.receiveShadow = true; boardGroup.add(frameM);
[[0, 3.83, 7.72, 0.06], [0, -3.83, 7.72, 0.06], [3.83, 0, 0.06, 7.72], [-3.83, 0, 0.06, 7.72]].forEach(([x, z, w, d]) => { const t = new THREE.Mesh(new THREE.BoxGeometry(w, 0.04, d), goldMat); t.position.set(x, 0.06, z); boardGroup.add(t); });
[[0, 4.47, 9.0, 0.06], [0, -4.47, 9.0, 0.06], [4.47, 0, 0.06, 9.0], [-4.47, 0, 0.06, 9.0]].forEach(([x, z, w, d]) => { const t = new THREE.Mesh(new THREE.BoxGeometry(w, 0.04, d), goldMat); t.position.set(x, 0.06, z); boardGroup.add(t); });
// coordinate a–h, 1–8 incise sulla cornice
const coordMats = {};
function coordMat(ch) { if (coordMats[ch]) return coordMats[ch]; const c = mkCanvas(64), g = c.getContext('2d'); g.fillStyle = '#e8d6ae'; g.font = '600 44px Cinzel, Georgia, serif'; g.textAlign = 'center'; g.textBaseline = 'middle'; g.fillText(ch, 32, 34);
  const t = new THREE.CanvasTexture(c); t.encoding = THREE.sRGBEncoding; return coordMats[ch] = new THREE.MeshBasicMaterial({ map: t, transparent: true, depthWrite: false, fog: false, polygonOffset: true, polygonOffsetFactor: -4, polygonOffsetUnits: -4 }); }
const coordGeo = new THREE.PlaneGeometry(0.32, 0.32);
for (let i = 0; i < 8; i++) { const pos = i * 0.95 - 3.325;
  [[String.fromCharCode(97 + i), pos, 4.17, 0], [String.fromCharCode(97 + i), pos, -4.17, Math.PI], [String(8 - i), -4.17, pos, 0], [String(8 - i), 4.17, pos, Math.PI]].forEach(([ch, x, z, r]) => {
    const m = new THREE.Mesh(coordGeo, coordMat(ch)); m.rotation.x = -Math.PI / 2; m.rotation.z = r; m.position.set(x, 0.07, z); m.renderOrder = 2; boardGroup.add(m); }); }
for (let i = 1; i < 8; i++) { const p = i * 0.95 - 3.8;           // filetti d'oro tra le case
  [[p, 0, 0.012, 7.6], [0, p, 7.6, 0.012]].forEach(([x, z, w, d]) => { const t = new THREE.Mesh(new THREE.BoxGeometry(w, 0.157, d), goldMat); t.position.set(x, 0.0, z); boardGroup.add(t); }); }
[[-1, -1], [1, -1], [-1, 1], [1, 1]].forEach(([sx, sz]) => { const g = new THREE.Group(); g.position.set(sx * 4.47, 0.05, sz * 4.47); boardGroup.add(g);
  const b = new THREE.Mesh(new THREE.BoxGeometry(0.22, 0.22, 0.22), frameMat); b.position.y = 0.11; b.castShadow = true; g.add(b);
  const c = new THREE.Mesh(new THREE.ConeGeometry(0.13, 0.34, 4), frameMat); c.position.y = 0.39; c.rotation.y = Math.PI / 4; c.castShadow = true; g.add(c);
  const k = new THREE.Mesh(new THREE.SphereGeometry(0.035, 10, 8), goldMat); k.position.y = 0.58; g.add(k);
  const r = new THREE.Mesh(new THREE.TorusGeometry(0.115, 0.012, 6, 4), goldMat); r.rotation.x = Math.PI / 2; r.rotation.z = Math.PI / 4; r.position.y = 0.225; g.add(r); });
const boardFill = new THREE.PointLight(0xffe2b8, 0.55, 11, 2); boardFill.position.set(0, 4.5, 2); scene.add(boardFill);
const tileSize = 0.95, boardOffset = 8 * tileSize / 2 - tileSize / 2, tiles = {};
const tileGeo = new THREE.BoxGeometry(tileSize - 0.004, 0.15, tileSize - 0.004);
for (let row = 0; row < 8; row++) for (let col = 0; col < 8; col++) {
  const t = new THREE.Mesh(tileGeo, (row + col) % 2 ? blackTileMat : whiteTileMat); t.position.set(col * tileSize - boardOffset, 0, row * tileSize - boardOffset); t.receiveShadow = true;
  const sq = String.fromCharCode(97 + col) + (8 - row); t.userData = { square: sq }; boardGroup.add(t); tiles[sq] = t;
}
const highlightMats = {
  select: new THREE.MeshBasicMaterial({ color: 0xd4a017, transparent: true, opacity: 0.55, depthWrite: false, polygonOffset: true, polygonOffsetFactor: -2, polygonOffsetUnits: -2 }),
  move: new THREE.MeshBasicMaterial({ color: 0x3a9a5a, transparent: true, opacity: 0.5, depthWrite: false, polygonOffset: true, polygonOffsetFactor: -2, polygonOffsetUnits: -2 }),
  capture: new THREE.MeshBasicMaterial({ color: 0xb02020, transparent: true, opacity: 0.6, depthWrite: false, polygonOffset: true, polygonOffsetFactor: -2, polygonOffsetUnits: -2 }),
  last: new THREE.MeshBasicMaterial({ color: 0xa8854f, transparent: true, opacity: 0.28, depthWrite: false, polygonOffset: true, polygonOffsetFactor: -2, polygonOffsetUnits: -2 }) };
const activeHighlights = [], lastMarks = [], hlGeo = new THREE.PlaneGeometry(tileSize * 0.94, tileSize * 0.94);
function clearHighlights() { activeHighlights.forEach(h => scene.remove(h)); activeHighlights.length = 0; }
function mark(sq, type, list) { const t = tiles[sq]; if (!t) return; const h = new THREE.Mesh(hlGeo, highlightMats[type]); h.rotation.x = -Math.PI / 2; h.position.set(t.position.x, type === 'last' ? 0.081 : 0.086, t.position.z); h.renderOrder = type === 'last' ? 1 : 3; scene.add(h); list.push(h); }
function addHighlight(sq, type) { if (!S.highlights && type !== 'select') return; mark(sq, type, activeHighlights); }
function updateLastMarks() { lastMarks.forEach(h => scene.remove(h)); lastMarks.length = 0; if (S.lastmove && lastMove) { mark(lastMove.from, 'last', lastMarks); mark(lastMove.to, 'last', lastMarks); }
  checkSquare = game.in_check() ? findKing(game.turn()) : null; if (checkSquare) mark(checkSquare, 'capture', lastMarks); }

const pieceObjects = {};
function spawnPiece(type, color, sq) {
  const mesh = new THREE.Group(), cast = renderer.shadowMap.enabled, geos = pieceGeometries[type];
  Object.entries(geos).forEach(([r, geo]) => { const m = new THREE.Mesh(geo, pieceMats[color][r]); m.castShadow = cast && r !== 'inset'; m.receiveShadow = r === 'body' || r === 'accent'; mesh.add(m); });
  const t = tiles[sq]; mesh.position.set(t.position.x, 0.08, t.position.z); if (color === 'b') mesh.rotation.y = Math.PI;
  scene.add(mesh); pieceObjects[sq] = mesh;
}
function renderBoardState() {
  Object.keys(pieceObjects).forEach(sq => { scene.remove(pieceObjects[sq]); delete pieceObjects[sq]; });
  const b = game.board(); for (let r = 0; r < 8; r++) for (let c = 0; c < 8; c++) if (b[r][c]) spawnPiece(b[r][c].type, b[r][c].color, String.fromCharCode(97 + c) + (8 - r));
  toppling = null; if (selectedSquare) setSelected(selectedSquare); else setSelected(null);
  updateLastMarks(); updateUI();
}
const sparks = [], dying = [];
let selectedMesh = null, toppling = null, checkSquare = null;
const selRing = new THREE.Mesh(new THREE.RingGeometry(0.33, 0.43, 40), new THREE.MeshBasicMaterial({ color: 0xffc860, transparent: true, opacity: 0.7, blending: THREE.AdditiveBlending, depthWrite: false, side: THREE.DoubleSide, polygonOffset: true, polygonOffsetFactor: -4, polygonOffsetUnits: -4 }));
selRing.rotation.x = -Math.PI / 2; selRing.visible = false; selRing.renderOrder = 4; scene.add(selRing);
const checkLight = new THREE.PointLight(0xff3020, 0, 3.5, 2); scene.add(checkLight);
function setSelected(sq) { selectedMesh = sq ? pieceObjects[sq] || null : null; selRing.visible = !!selectedMesh; if (selectedMesh) selRing.position.set(tiles[sq].position.x, 0.09, tiles[sq].position.z); }
function dissolvePiece(mesh) {
  if (!mesh) return; const mats = [];
  mesh.traverse(c => { if (c.isMesh) { c.material = c.material.clone(); c.material.transparent = true; c.material.depthWrite = false; c.material.emissive && c.material.emissive.setHex(0x401808); c.castShadow = false; mats.push(c.material); } });
  dying.push({ mesh, mats, t: 0, spin: (Math.random() - 0.5) * 5 });
  burst(mesh.position.x, mesh.position.z, 0xff7030); burst(mesh.position.x, mesh.position.z, 0x8a6a50);
}
function findKing(color) { const b = game.board(); for (let r = 0; r < 8; r++) for (let c = 0; c < 8; c++) { const p = b[r][c]; if (p && p.type === 'k' && p.color === color) return String.fromCharCode(97 + c) + (8 - r); } return null; }
function burst(x, z, color) { for (let i = 0; i < 16; i++) { const s = makeGlow(color, 0.25, 1); s.position.set(x, 0.2, z); scene.add(s); sparks.push({ s, v: new THREE.Vector3((Math.random() - 0.5) * 0.07, 0.03 + Math.random() * 0.05, (Math.random() - 0.5) * 0.07), life: 1 }); } }
function animatePieceMove(from, to, cb) {
  const m = pieceObjects[from]; if (!m) { cb && cb(); return; }
  m.userData.moving = true; const tt = tiles[to], sp = m.position.clone(), ep = new THREE.Vector3(tt.position.x, 0.08, tt.position.z); let p = 0; const dur = Math.round(22 / S.animSpeed);
  (function a() { p++; const t = p / dur, e = t < 0.5 ? 2 * t * t : 1 - Math.pow(-2 * t + 2, 2) / 2; m.position.lerpVectors(sp, ep, e); m.position.y = 0.08 + Math.sin(t * Math.PI) * 0.7;
    m.rotation.x = Math.sin(t * Math.PI) * 0.12 * (ep.z > sp.z ? 1 : -1);
    if (p < dur) requestAnimationFrame(a); else { m.position.copy(ep); m.rotation.x = 0; m.userData.moving = false; cb && cb(); } })();
}

// =====================================================================
// SCACCHI DEI MAGHI: ogni pezzo attacca a modo suo, la vittima va in frantumi
// =====================================================================
const tw = (dur, fn) => new Promise(res => { const t0 = performance.now(), D = Math.max(16, dur * 1000 / S.animSpeed);
  (function st() { const p = Math.min(1, (performance.now() - t0) / D); fn(p); if (p < 1) requestAnimationFrame(st); else res(); })(); });
const wait = s => tw(s, () => {});
const eio = p => p < 0.5 ? 2 * p * p : 1 - Math.pow(-2 * p + 2, 2) / 2, eout = p => 1 - Math.pow(1 - p, 3), ein = p => p * p * p;
let moveBusy = false, camShake = 0;
const _qY = new THREE.Quaternion(), _qT = new THREE.Quaternion(), _UP = new THREE.Vector3(0, 1, 0);
function pose(m, yaw, dir, tilt) { _qY.setFromAxisAngle(_UP, yaw); const ax = new THREE.Vector3(dir.z, 0, -dir.x).normalize(); _qT.setFromAxisAngle(ax, tilt); m.quaternion.copy(_qT).multiply(_qY); }
const sideColor = c => c === 'w' ? new THREE.Color(1.0, 0.75, 0.35) : new THREE.Color(1.0, 0.18, 0.06);
// frammenti con fisica
const shards = [], shardGeos = [new THREE.TetrahedronGeometry(1), new THREE.DodecahedronGeometry(1), new THREE.OctahedronGeometry(1), new THREE.BoxGeometry(1.2, 0.7, 0.9)];
const fxRings = [];
function floorAt(x, z) { return (Math.abs(x) < 4.5 && Math.abs(z) < 4.5) ? 0.085 : (Math.abs(x) < 5.5 && Math.abs(z) < 5.5) ? PL : FY; }
function shatter(mesh, color, type, dir) {
  if (!mesh) return; const pos = mesh.position.clone(); scene.remove(mesh);
  const H = { p: 0.95, r: 1.15, n: 1.15, b: 1.4, q: 1.6, k: 1.8 }[type] || 1.2, n = IS_MOBILE ? 26 : 42;
  for (let i = 0; i < n; i++) {
    const big = Math.random() < 0.25, s = (big ? 0.07 : 0.03) + Math.random() * (big ? 0.06 : 0.04);
    const mat = Math.random() < 0.82 ? pieceMats[color].body : (Math.random() < 0.7 ? pieceMats[color].accent : pieceMats[color].inset);
    const m = new THREE.Mesh(shardGeos[(Math.random() * shardGeos.length) | 0], mat); m.scale.set(s, s * (0.6 + Math.random()), s * (0.6 + Math.random() * 0.8));
    const y = 0.15 + Math.random() * H, rr = Math.random() * 0.22 * (1 - y / (H + 0.3)) + 0.04, a = Math.random() * 6.283;
    m.position.set(pos.x + Math.cos(a) * rr, y, pos.z + Math.sin(a) * rr); m.castShadow = true; m.rotation.set(Math.random() * 6, Math.random() * 6, Math.random() * 6);
    const out = new THREE.Vector3(Math.cos(a), 0, Math.sin(a));
    const v = out.multiplyScalar(0.8 + Math.random() * 1.6).add(dir.clone().multiplyScalar(1.2 + Math.random() * 1.4)); v.y = 1.0 + Math.random() * 2.8;
    scene.add(m); shards.push({ m, v, w: new THREE.Vector3((Math.random() - 0.5) * 14, (Math.random() - 0.5) * 14, (Math.random() - 0.5) * 14), life: 2.4 + Math.random() * 0.8, s0: m.scale.clone(), rest: false });
  }
  burst(pos.x, pos.z, color === 'w' ? 0xffc070 : 0xff4020); burst(pos.x, pos.z, 0xa08a70);
  for (let i = 0; i < 6; i++) { const sp = smokePuff(); scene.add(sp); sp.position.set(pos.x + (Math.random() - 0.5) * 0.3, 0.2 + Math.random() * 0.5, pos.z + (Math.random() - 0.5) * 0.3); sp.material.color.setHex(0x9a9088);
    shards.push({ puff: sp, t: 0, d: 0.9 + Math.random() * 0.6 }); }
  ring(pos, color, 1.4); audio.crack();
}
function ring(pos, color, size) {
  const m = new THREE.Mesh(new THREE.RingGeometry(0.2, 0.32, 48), new THREE.MeshBasicMaterial({ color: sideColor(color), transparent: true, opacity: 0.9, blending: THREE.AdditiveBlending, depthWrite: false, side: THREE.DoubleSide }));
  m.rotation.x = -Math.PI / 2; m.position.set(pos.x, 0.1, pos.z); m.renderOrder = 6; scene.add(m); fxRings.push({ m, t: 0, size });
}
function updateShards(dt) {
  for (let i = shards.length - 1; i >= 0; i--) { const s = shards[i];
    if (s.puff) { s.t += dt / s.d; s.puff.position.y += dt * 0.5; s.puff.scale.setScalar(0.4 + s.t * 1.6); s.puff.material.opacity = 0.35 * Math.sin(Math.min(1, s.t) * Math.PI);
      if (s.t >= 1) { scene.remove(s.puff); s.puff.material.dispose(); shards.splice(i, 1); } continue; }
    s.life -= dt;
    if (!s.rest) { s.v.y -= 9.8 * dt; s.m.position.addScaledVector(s.v, dt); s.m.rotation.x += s.w.x * dt; s.m.rotation.y += s.w.y * dt; s.m.rotation.z += s.w.z * dt;
      const fl = floorAt(s.m.position.x, s.m.position.z) + s.m.scale.y * 0.5;
      if (s.m.position.y < fl) { s.m.position.y = fl; if (Math.abs(s.v.y) < 0.6) { s.rest = true; } else { s.v.y *= -0.32; s.v.x *= 0.55; s.v.z *= 0.55; s.w.multiplyScalar(0.5); } } }
    if (s.life < 0.6) s.m.scale.copy(s.s0).multiplyScalar(Math.max(0.001, s.life / 0.6));
    if (s.life <= 0) { scene.remove(s.m); shards.splice(i, 1); } }
  for (let i = fxRings.length - 1; i >= 0; i--) { const r = fxRings[i]; r.t += dt / 0.7; const k = 1 + eout(Math.min(1, r.t)) * r.size * 4; r.m.scale.set(k, k, k); r.m.material.opacity = 0.9 * (1 - r.t);
    if (r.t >= 1) { scene.remove(r.m); r.m.geometry.dispose(); r.m.material.dispose(); fxRings.splice(i, 1); } }
}
// dardo magico (alfiere, regina)
function bolt(from, to, color) {
  const c = sideColor(color), core = makeGlow(c, 0.55, 1), halo = makeGlow(c, 1.3, 0.5); scene.add(core, halo);
  const mid = from.clone().lerp(to, 0.5); mid.y += 0.9 + from.distanceTo(to) * 0.12; const curve = new THREE.QuadraticBezierCurve3(from, mid, to);
  audio.zap(color);
  return tw(0.38, p => { const e = ein(p) * 0.6 + p * 0.4, q = curve.getPoint(e); core.position.copy(q); halo.position.copy(q);
    if (Math.random() < 0.7) { const sp = makeGlow(c, 0.18, 0.9); sp.position.copy(q); scene.add(sp); sparks.push({ s: sp, v: new THREE.Vector3((Math.random() - 0.5) * 0.02, -0.005, (Math.random() - 0.5) * 0.02), life: 0.7 }); } })
    .then(() => { scene.remove(core, halo); core.material.dispose(); halo.material.dispose(); });
}
async function playMove(mv, capMesh, capSq) {
  const m = pieceObjects[mv.from]; if (!m) return;
  m.userData.moving = true; const type = mv.piece, color = mv.color, yaw0 = color === 'b' ? Math.PI : 0;
  const sp = m.position.clone(); sp.y = 0.08; const tt = tiles[mv.to], ep = new THREE.Vector3(tt.position.x, 0.08, tt.position.z);
  const dir = ep.clone().sub(sp); dir.y = 0; const dist = dir.length(); dir.normalize();
  const yawN = type === 'n' ? Math.atan2(dir.z, -dir.x) : yaw0;           // il cavallo si gira verso la meta
  castMagic(color, m, capMesh ? 1 : 0.55);
  // arrocco: la torre scivola insieme al re
  let rookP = Promise.resolve();
  if (mv.flags.includes('k') || mv.flags.includes('q')) { const rank = mv.from[1], ks = mv.flags.includes('k'), rm = pieceObjects[(ks ? 'h' : 'a') + rank];
    if (rm) { rm.userData.moving = true; const a = rm.position.clone(), b = tiles[(ks ? 'f' : 'd') + rank].position.clone(); b.y = 0.08;
      rookP = wait(0.25).then(() => tw(0.8, p => { rm.position.lerpVectors(a, b, eio(p)); rm.position.y = 0.08 + Math.sin(p * Math.PI) * 0.35; })).then(() => { rm.userData.moving = false; }); } }
  if (type === 'n') await tw(0.18, p => pose(m, yaw0 + (yawN - yaw0) * eio(p), dir, 0));
  if (!capMesh) {
    if (type === 'n') await tw(0.8, p => { const e = eio(p); m.position.lerpVectors(sp, ep, e); m.position.y = 0.08 + Math.sin(p * Math.PI) * 1.05; pose(m, yawN, dir, Math.sin(p * Math.PI * 2) * -0.25); });
    else await tw(0.3 + dist * 0.1, p => { const e = eio(p); m.position.lerpVectors(sp, ep, e); m.position.y = 0.08 + Math.sin(p * Math.PI) * (0.14 + dist * 0.03); pose(m, yawN, dir, Math.sin(p * Math.PI) * 0.1); });
    await tw(0.14, p => { m.position.y = 0.08 - Math.sin(p * Math.PI) * 0.02; });
    await rookP; return;
  }
  // ---------- cattura ----------
  const vp = capMesh.position.clone(); vp.y = 0.08; const vdir = vp.clone().sub(sp); vdir.y = 0; const vdist = vdir.length(); vdir.normalize();
  const stop = vp.clone().sub(vdir.clone().multiplyScalar(Math.min(0.6, vdist * 0.5)));
  const vColor = color === 'w' ? 'b' : 'w', vType = (game.get(capSq) || {}).type || 'p';
  const tremble = () => tw(0.16, p => { capMesh.position.x = vp.x + Math.sin(p * 60) * 0.02 * (1 - p); capMesh.position.z = vp.z + Math.cos(p * 55) * 0.02 * (1 - p); });
  if (type === 'b' || type === 'q') {
    if (type === 'q') await tw(0.45, p => { pose(m, yaw0 + eio(p) * Math.PI * 2, dir, 0); m.position.y = 0.08 + Math.sin(p * Math.PI) * 0.3; });
    else await tw(0.25, p => { pose(m, yaw0, vdir, -Math.sin(p * Math.PI) * 0.18); });
    const top = m.position.clone(); top.y = type === 'q' ? 1.6 : 1.4; const hit = vp.clone(); hit.y = 0.6;
    if (type === 'q') { bolt(top.clone().add(new THREE.Vector3(0.2, 0, 0)), hit, color); await wait(0.12); bolt(top.clone().add(new THREE.Vector3(-0.2, 0, 0)), hit, color); await wait(0.12); }
    await bolt(top, hit, color); await tremble(); shatter(capMesh, vColor, vType, vdir); camShake = 0.12;
    await tw(0.3 + vdist * 0.1, p => { const e = eio(p); m.position.lerpVectors(sp, ep, e); m.position.y = 0.08 + Math.sin(p * Math.PI) * 0.18; pose(m, yaw0, vdir, Math.sin(p * Math.PI) * 0.08); });
  } else {
    if (type === 'n') await tw(0.7, p => { m.position.lerpVectors(sp, stop, eio(p)); m.position.y = 0.08 + Math.sin(p * Math.PI) * 1.0; pose(m, yawN, vdir, Math.sin(p * Math.PI) * -0.2); });
    else await tw(0.3 + vdist * 0.08, p => { m.position.lerpVectors(sp, stop, eio(p)); m.position.y = 0.08 + Math.sin(p * Math.PI) * 0.08; });
    const at = stop.clone();
    if (type === 'p') { await tw(0.22, p => { const e = eout(p); m.position.copy(at).addScaledVector(vdir, -0.14 * e); pose(m, yawN, vdir, -0.25 * e); });
      await tw(0.1, p => { const e = ein(p); m.position.copy(at).addScaledVector(vdir, -0.14 + 0.36 * e); pose(m, yawN, vdir, -0.25 + 0.6 * e); }); }
    else if (type === 'n') { await tw(0.3, p => { const e = eout(p); m.position.y = 0.08 + 0.25 * e; pose(m, yawN, vdir, -0.7 * e); });
      await tw(0.12, p => { const e = ein(p); m.position.copy(at).addScaledVector(vdir, 0.18 * e); m.position.y = 0.33 - 0.25 * e; pose(m, yawN, vdir, -0.7 + 1.05 * e); }); camShake = 0.18; }
    else if (type === 'r') { await tw(0.32, p => { const e = eout(p); m.position.copy(at).addScaledVector(vdir, 0.15 * e); m.position.y = 0.08 + 0.95 * e; pose(m, yawN, vdir, -0.15 * e); });
      await tw(0.11, p => { const e = ein(p); m.position.copy(at).addScaledVector(vdir, 0.15 + 0.12 * e); m.position.y = 1.03 - 0.95 * e; pose(m, yawN, vdir, -0.15 + 0.25 * e); }); camShake = 0.3; ring(vp, color, 2.2); }
    else { await tw(0.36, p => { const e = eout(p); m.position.y = 0.08 + 0.3 * e; pose(m, yawN, vdir, -0.4 * e); });                 // re
      await tw(0.12, p => { const e = ein(p); m.position.copy(at).addScaledVector(vdir, 0.2 * e); m.position.y = 0.38 - 0.3 * e; pose(m, yawN, vdir, -0.4 + 0.75 * e); }); camShake = 0.25; ring(vp, color, 1.8); }
    shatter(capMesh, vColor, vType, vdir);
    await tw(0.18, p => pose(m, yawN, vdir, 0.35 * (1 - eout(p)) * (type === 'r' ? 0.3 : 1)));
    const from2 = m.position.clone(); from2.y = 0.08;
    await tw(0.32, p => { const e = eio(p); m.position.lerpVectors(from2, ep, e); m.position.y = 0.08 + Math.sin(p * Math.PI) * 0.1; });
  }
  if (type === 'n') await tw(0.2, p => pose(m, yawN + (yaw0 - yawN) * eio(p), dir, 0));
  await rookP;
}

// =====================================================================
// INTERAZIONE
// =====================================================================
const raycaster = new THREE.Raycaster(), mouse = new THREE.Vector2();
function getSquare(e) { mouse.x = e.clientX / innerWidth * 2 - 1; mouse.y = -(e.clientY / innerHeight) * 2 + 1; raycaster.setFromCamera(mouse, camera);
  for (const i of raycaster.intersectObjects(boardGroup.children)) if (i.object.userData.square) return i.object.userData.square; return null; }
function handleSquareClick(sq) {
  if (moveBusy || game.game_over() || isAiThinking || (gameMode === 'ai' && game.turn() === 'b')) return;
  if (selectedSquare) { if (selectedSquare === sq) { selectedSquare = null; setSelected(null); clearHighlights(); return; } const mv = legalMoves.find(m => m.to === sq); if (mv) { executeMove(mv); return; } }
  const p = game.get(sq);
  if (p && p.color === game.turn()) { selectedSquare = sq; setSelected(sq); clearHighlights(); addHighlight(sq, 'select'); legalMoves = game.moves({ square: sq, verbose: true }); legalMoves.forEach(m => addHighlight(m.to, m.captured ? 'capture' : 'move')); }
  else { selectedSquare = null; setSelected(null); clearHighlights(); }
}
let pendingPromotionMove = null;
async function executeMove(mv) {
  if (moveBusy) return;
  if (!mv.promotion && (mv.flags.includes('p') || (mv.piece === 'p' && (mv.to[1] === '8' || mv.to[1] === '1')))) {
    if (gameMode === 'ai' && mv.color === 'b') mv.promotion = 'q'; else { pendingPromotionMove = mv; document.getElementById('promotion-modal').classList.remove('hidden'); closeMenu(); return; } }
  moveBusy = true; const cap = !!mv.captured; clearHighlights(); selectedSquare = null; setSelected(null);
  let capMesh = null, cs = null; if (cap) { cs = mv.flags.includes('e') ? mv.to[0] + mv.from[1] : mv.to; capMesh = pieceObjects[cs]; delete pieceObjects[cs]; }
  try { await playMove(mv, capMesh, cs); } catch (e) { console.error(e); }
  if (capMesh && capMesh.parent) scene.remove(capMesh);
  const res = game.move({ from: mv.from, to: mv.to, promotion: mv.promotion }); moveBusy = false;
  if (!res) { renderBoardState(); return; }
  lastMove = { from: mv.from, to: mv.to };
  if (!cap) audio.playMove();
  document.getElementById('bottom-bar').classList.toggle('in-check', game.in_check()); if (game.in_check()) audio.playCheck();
  renderBoardState();
  if (isCinemaMode && !game.game_over()) setCameraPreset(game.turn() === 'w' ? 'white' : 'black');
  if (game.in_checkmate()) { const ks = findKing(game.turn()); if (ks && pieceObjects[ks]) toppling = { mesh: pieceObjects[ks], t: 0, dir: Math.random() > 0.5 ? 1 : -1 }; }
  if (game.game_over()) setTimeout(handleGameOverCondition, game.in_checkmate() ? 1400 : 0); else if (gameMode === 'ai' && game.turn() === 'b') triggerAiTurn();
}
function triggerAiTurn() { isAiThinking = true; document.getElementById('game-status').innerText = 'Nero (IA) riflette…'; setTimeout(() => { const m = getBestMoveAI(); isAiThinking = false; if (m) executeMove(m); }, 600); }
function handleGameOverCondition() {
  clearInterval(timerInterval); let title = 'Partita terminata', desc = 'La partita è giunta a conclusione.';
  if (game.in_checkmate()) { const w = game.turn() === 'w' ? 'Nero' : 'Bianco'; title = 'Scacco matto!'; desc = `Il ${w} trionfa nella cattedrale.`; if (typeof confetti === 'function') confetti({ particleCount: 90, spread: 60, origin: { y: 0.6 }, colors: ['#a8854f', '#5c4426', '#d8c8a8'] }); }
  else if (game.in_draw()) { title = 'Patta'; desc = 'Le forze si equivalgono.'; }
  triggerGameOver(title, desc);
}
function triggerGameOver(t, d) { audio.bell(); document.getElementById('gameover-title').innerText = t; document.getElementById('gameover-desc').innerText = d; closeMenu(); document.getElementById('gameover-modal').classList.remove('hidden'); }
let downPos = null;
renderer.domElement.addEventListener('pointerdown', e => { downPos = { x: e.clientX, y: e.clientY }; controls.autoRotate = false; });
renderer.domElement.addEventListener('pointerup', e => { controls.autoRotate = S.autorotate; if (!downPos || Math.hypot(e.clientX - downPos.x, e.clientY - downPos.y) > 8) return; const sq = getSquare(e); if (sq) handleSquareClick(sq); });

const pieceIcons = { p: '♟', r: '♜', n: '♞', b: '♝', q: '♛', k: '♚' };
function updateUI() {
  const s = document.getElementById('game-status'), c = document.getElementById('check-status'), dot = document.getElementById('turn-dot');
  dot.className = 'dot ' + game.turn();
  if (game.in_checkmate()) { s.innerText = `Matto, vince il ${game.turn() === 'w' ? 'Nero' : 'Bianco'}`; s.className = 'mate'; }
  else if (game.in_draw()) { s.innerText = 'Patta'; s.className = 'draw'; }
  else { s.innerText = game.turn() === 'w' ? 'Bianco' : 'Nero'; s.className = ''; }
  if (game.in_check() && !game.in_checkmate()) { c.innerText = 'Scacco!'; c.className = 'sb-check alert'; } else { c.innerText = ''; c.className = 'sb-check hidden'; }
  document.getElementById('bottom-bar').classList.toggle('in-check', game.in_check());
  const hv = game.history({ verbose: true }), h = game.history();
  document.getElementById('last-move').innerText = h.length ? `${Math.ceil(h.length / 2)}. ${h.length % 2 ? '' : '… '}${h[h.length - 1]}` : '';
  const cw = [], cb = []; hv.forEach(m => { if (m.captured) (m.color === 'w' ? cb : cw).push(m.captured); });
  document.getElementById('graveyard-white').innerText = cw.map(p => pieceIcons[p]).join(' ');
  document.getElementById('graveyard-black').innerText = cb.map(p => pieceIcons[p]).join(' ');
  const log = document.getElementById('move-log'); log.innerHTML = h.length ? '' : '<div class="hint">Nessuna mossa ancora. Muove il Bianco.</div>';
  for (let i = 0; i < h.length; i += 2) { const r = document.createElement('div'); r.className = 'row'; r.innerHTML = `<span class="n">${i / 2 + 1}.</span><span>${h[i]}</span><span>${h[i + 1] || ''}</span>`; log.appendChild(r); }
  updateTimerUI();
}

// =====================================================================
// TEMI E APPLICAZIONE IMPOSTAZIONI
// =====================================================================
const THEMES = {
  sanctum: { bg: 0x060404, amb: [0x1c1724, 0.32], hemi: [0x2a2840, 0x0a0806, 0.22], moon: [0x7d8fbf, 0.22], spot: [0xffc27a, 3.4], flame: 0xffa040, glass: 0.62, shaft: 0xbfd0ff },
  moonlight: { bg: 0x02040a, amb: [0x121a2a, 0.38], hemi: [0x26365a, 0x05060a, 0.28], moon: [0xa8c4ff, 0.8], spot: [0xd8e4ff, 3.0], flame: 0xffb070, glass: 0.8, shaft: 0xcfe0ff },
  vespers: { bg: 0x07040a, amb: [0x241a2c, 0.38], hemi: [0x4a2a50, 0x0a0606, 0.28], moon: [0xff9050, 0.55], spot: [0xffb060, 3.2], flame: 0xffa040, glass: 1.05, shaft: 0xffb080 },
 };
let themeBase = null;
function applyTheme() {
  if (!THEMES[S.theme]) S.theme = 'sanctum'; const t = THEMES[S.theme]; themeBase = t; S.storm = S.theme === 'moonlight';
  document.querySelectorAll('.theme-btn').forEach(b => b.classList.toggle('active', b.dataset.theme === S.theme));
  scene.background.setHex(t.bg); scene.fog.color.setHex(t.bg);
  ambientLight.color.setHex(t.amb[0]); ambientLight.intensity = t.amb[1];
  hemi.color.setHex(t.hemi[0]); hemi.groundColor.setHex(t.hemi[1]); hemi.intensity = t.hemi[2];
  moon.color.setHex(t.moon[0]); moon.intensity = t.moon[1]; boardSpot.color.setHex(t.spot[0]); boardSpot.intensity = t.spot[1];
  flameMat.color.setHex(t.flame).multiplyScalar(2.0); flames.forEach(f => f.glow.material.color.setHex(t.flame)); flickerLights.forEach(f => f.light.color.setHex(t.flame));
  glassMats.forEach(m => m.color.setScalar(t.glass)); shafts.forEach(s => s.mesh.material.color.setHex(t.shaft));
}
let envTex = null, envTimer = null;
// Riflessi ambientali (PMREM) disattivati: su alcune schede video rendevano neri marmo, oro e pezzi.
function buildEnv() {}
// ---------- Post-processing cinematografico: HDR, bloom, tone mapping ACES, grading e vignettatura ----------
let composer = null, finalPass = null;
const FinalShader = {
  uniforms: { tDiffuse: { value: null }, exposure: { value: 1.0 }, vignette: { value: 1.15 }, time: { value: 0 } },
  vertexShader: 'varying vec2 vUv; void main(){ vUv = uv; gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0); }',
  fragmentShader: `uniform sampler2D tDiffuse; uniform float exposure; uniform float vignette; uniform float time; varying vec2 vUv;
    vec3 sc_rrt(vec3 v){ vec3 a = v * (v + 0.0245786) - 0.000090537; vec3 b = v * (0.983729 * v + 0.4329510) + 0.238081; return a / b; }
    vec3 sc_aces(vec3 c){ const mat3 I = mat3(vec3(0.59719,0.07600,0.02840), vec3(0.35458,0.90834,0.13383), vec3(0.04823,0.01566,0.83777));
      const mat3 O = mat3(vec3(1.60475,-0.10208,-0.00327), vec3(-0.53108,1.10813,-0.07276), vec3(-0.07367,-0.00605,1.07602));
      c *= exposure / 0.6; c = I * c; c = sc_rrt(c); c = O * c; return clamp(c, 0.0, 1.0); }
    vec3 sc_srgb(vec3 c){ return mix(pow(c, vec3(0.41666)) * 1.055 - vec3(0.055), c * 12.92, vec3(lessThanEqual(c, vec3(0.0031308)))); }
    void main(){ vec3 c = texture2D(tDiffuse, vUv).rgb; c = sc_aces(c);
      float l = dot(c, vec3(0.2126, 0.7152, 0.0722));
      c = mix(c * vec3(0.93, 0.98, 1.08), c * vec3(1.06, 1.0, 0.9), smoothstep(0.05, 0.6, l));      // ombre fredde, luci calde
      c = mix(vec3(l), c, 1.08);                                                                     // un filo di saturazione
      c = sc_srgb(c);
      vec2 d = vUv - 0.5; c *= clamp(1.0 - dot(d, d) * vignette, 0.0, 1.0);
      float n = fract(sin(dot(vUv * 1024.0 + time, vec2(12.9898, 78.233))) * 43758.5453); c += (n - 0.5) / 255.0;
      gl_FragColor = vec4(c, 1.0); }` };
function halfFloatWorks() {
  const cam = new THREE.OrthographicCamera(-1, 1, 1, -1, 0, 1), b = new THREE.WebGLRenderTarget(4, 4);
  const a = THREE.WebGLMultisampleRenderTarget ? new THREE.WebGLMultisampleRenderTarget(4, 4, { type: THREE.HalfFloatType }) : new THREE.WebGLRenderTarget(4, 4, { type: THREE.HalfFloatType }); if ('samples' in a) a.samples = IS_MOBILE ? 2 : 4;
  try {
    const s1 = new THREE.Scene(); s1.add(new THREE.Mesh(new THREE.PlaneGeometry(2, 2), new THREE.MeshBasicMaterial({ color: new THREE.Color(0.5, 0.25, 0.125) })));
    renderer.setRenderTarget(a); renderer.render(s1, cam);
    const s2 = new THREE.Scene(); s2.add(new THREE.Mesh(new THREE.PlaneGeometry(2, 2), new THREE.MeshBasicMaterial({ map: a.texture })));
    renderer.setRenderTarget(b); renderer.render(s2, cam);
    const px = new Uint8Array(4); renderer.readRenderTargetPixels(b, 1, 1, 1, 1, px); renderer.setRenderTarget(null);
    return Math.abs(px[0] - 128) < 14 && Math.abs(px[1] - 64) < 14 && Math.abs(px[2] - 32) < 14;
  } catch (e) { renderer.setRenderTarget(null); return false; } finally { a.dispose(); b.dispose(); }
}
function setupPost() {
  if (!THREE.EffectComposer || !THREE.UnrealBloomPass || !THREE.ShaderPass || !THREE.RenderPass) return false;
  if (!renderer.capabilities.isWebGL2 || !halfFloatWorks()) return false;
  try {
    const w = innerWidth || 1, h = innerHeight || 1;
    const rt = THREE.WebGLMultisampleRenderTarget ? new THREE.WebGLMultisampleRenderTarget(w, h, { type: THREE.HalfFloatType, format: THREE.RGBAFormat }) : new THREE.WebGLRenderTarget(w, h, { type: THREE.HalfFloatType });
    if ('samples' in rt) rt.samples = IS_MOBILE ? 2 : 4;
    composer = new THREE.EffectComposer(renderer, rt);
    composer.addPass(new THREE.RenderPass(scene, camera));
    composer.addPass(new THREE.UnrealBloomPass(new THREE.Vector2(w, h), IS_MOBILE ? 0.55 : 0.7, 0.6, 1.5));
    finalPass = new THREE.ShaderPass(FinalShader); finalPass.material.toneMapped = false; composer.addPass(finalPass);
    renderer.toneMapping = THREE.NoToneMapping;
    return true;
  } catch (e) { composer = null; return false; }
}
function applyEnv() { [goldMat, bronzeMat, ironMat, frameMat, whiteTileMat, blackTileMat, pieceMaterials.w, pieceMaterials.b].forEach(m => { if (m.envMap) { m.envMap = null; m.needsUpdate = true; } }); }
function scheduleEnv() { clearTimeout(envTimer); envTimer = setTimeout(buildEnv, 250); }
function targetPixelRatio() {
  const dpr = devicePixelRatio || 1, px = Math.max(1, innerWidth * innerHeight);
  return IS_MOBILE ? Math.min(dpr, 1.6, Math.sqrt(2.4e6 / px)) : Math.min(dpr, 2, Math.sqrt(6.0e6 / px));
}
function applySettings() {
  const pr = Math.max(0.75, targetPixelRatio()); renderer.setPixelRatio(pr); renderer.setSize(innerWidth || 1, innerHeight || 1);
  if (composer) { composer.setPixelRatio(pr); composer.setSize(innerWidth || 1, innerHeight || 1); }
  const shadowsOn = S.shadows !== 'off';
  if (renderer.shadowMap.enabled !== shadowsOn || (shadowsOn && renderer.shadowMap.type !== (S.shadows === 'soft' ? THREE.PCFSoftShadowMap : THREE.PCFShadowMap))) {
    renderer.shadowMap.enabled = shadowsOn; renderer.shadowMap.type = S.shadows === 'soft' ? THREE.PCFSoftShadowMap : THREE.PCFShadowMap;
    scene.traverse(o => { if (o.material) [].concat(o.material).forEach(m => m.needsUpdate = true); }); renderBoardState(); }
  boardSpot.castShadow = shadowsOn; moon.castShadow = shadowsOn && S.quality !== 'low';
  flickerLights.forEach(f => { if (f.optional) f.light.visible = S.quality !== 'low'; });
  applyEnv();
  renderer.toneMappingExposure = S.exposure; if (finalPass) finalPass.uniforms.exposure.value = S.exposure * 1.05; camera.fov = S.fov; applyViewOffset();
  controls.autoRotate = S.autorotate; scene.fog.density = S.fog;
  shaftG.visible = patchG.visible = S.shafts; dust.visible = smokeG.visible = S.dust; figuresG.visible = S.figures; creaturesG.visible = S.figures;
  mistG.visible = S.mist;
  audio.setVolumes(); updateLastMarks();
}
let currentTheme = S.theme;
function syncSettingsUI() { document.querySelectorAll('[data-key]').forEach(el => { const v = S[el.dataset.key]; if (el.type === 'checkbox') el.checked = !!v; else el.value = v; }); }
document.querySelectorAll('[data-key]').forEach(el => el.addEventListener(el.tagName === 'SELECT' || el.type === 'checkbox' ? 'change' : 'input', () => {
  const k = el.dataset.key; S[k] = el.type === 'checkbox' ? el.checked : el.type === 'range' ? parseFloat(el.value) : el.value;
  applySettings(); saveSettings(); }));
document.querySelectorAll('.tab-btn').forEach(b => b.onclick = () => { document.querySelector('.panels').scrollTop = 0; document.querySelectorAll('.tab-btn').forEach(x => x.classList.toggle('active', x === b)); document.querySelectorAll('[data-panel]').forEach(p => p.classList.toggle('hidden', p.dataset.panel !== b.dataset.tab)); });
document.querySelectorAll('.theme-btn').forEach(b => b.onclick = () => { S.theme = b.dataset.theme; applyTheme(); applySettings(); saveSettings(); });

// =====================================================================
// CONTROLLI UI
// =====================================================================
const $ = id => document.getElementById(id);
function openMenu() { syncSettingsUI(); $('drawer').classList.add('open'); $('drawer').setAttribute('aria-hidden', 'false'); $('scrim').classList.remove('hidden'); $('btn-menu').setAttribute('aria-expanded', 'true'); }
function closeMenu() { $('drawer').classList.remove('open'); $('drawer').setAttribute('aria-hidden', 'true'); $('scrim').classList.add('hidden'); $('btn-menu').setAttribute('aria-expanded', 'false'); }
const isNarrow = () => innerWidth <= 640;
$('btn-menu').onclick = () => $('drawer').classList.contains('open') ? closeMenu() : openMenu();
$('btn-close-menu').onclick = closeMenu; $('scrim').onclick = closeMenu;
addEventListener('keydown', e => {
  if (e.key === 'Escape') { closeMenu(); $('gameover-modal').classList.add('hidden'); }
  if ((e.key === 'm' || e.key === 'M') && !e.target.closest('input,select')) $('btn-menu').click();
});
function setActive(ids, on) { ids.forEach(x => $(x).classList.toggle('active', x === on)); }
$('btn-mode-pvp').onclick = () => { gameMode = 'pvp'; setActive(['btn-mode-pvp', 'btn-mode-ai'], 'btn-mode-pvp'); $('ai-difficulty-container').classList.add('hidden'); };
$('btn-mode-ai').onclick = () => { gameMode = 'ai'; setActive(['btn-mode-pvp', 'btn-mode-ai'], 'btn-mode-ai'); $('ai-difficulty-container').classList.remove('hidden'); if (game.turn() === 'b' && !game.game_over()) triggerAiTurn(); };
['easy', 'med', 'hard'].forEach(d => $(`ai-${d}`).onclick = () => { aiDifficulty = d; setActive(['ai-easy', 'ai-med', 'ai-hard'], `ai-${d}`); });
['off', '3m', '5m'].forEach(k => $(`timer-${k}`).onclick = () => { timerMode = k; setActive(['timer-off', 'timer-3m', 'timer-5m'], `timer-${k}`);
  const d = $('timer-display-container');
  if (k === 'off') { d.classList.add('hidden'); clearInterval(timerInterval); } else { d.classList.remove('hidden'); timeWhite = timeBlack = k === '3m' ? 180 : 300; updateTimerUI(); startTimerInterval(); }
  applyViewOffset(); });
$('btn-hint').onclick = () => { if (game.game_over() || isAiThinking || moveBusy) return; const m = getBestMoveAI(); if (m) { closeMenu(); clearHighlights(); mark(m.from, 'select', activeHighlights); mark(m.to, 'move', activeHighlights); setTimeout(clearHighlights, 3000); } };
$('btn-undo').onclick = () => { if (isAiThinking || moveBusy) return; game.undo(); if (gameMode === 'ai' && game.turn() === 'b') game.undo(); const h = game.history({ verbose: true }); lastMove = h.length ? h[h.length - 1] : null;
  selectedSquare = null; setSelected(null); clearHighlights(); renderBoardState(); };
$('btn-reset').onclick = () => { if (moveBusy) return; game.reset(); lastMove = null; selectedSquare = null; clearHighlights(); $('gameover-modal').classList.add('hidden');
  if (timerMode !== 'off') { timeWhite = timeBlack = timerMode === '3m' ? 180 : 300; startTimerInterval(); } renderBoardState(); closeMenu(); };
$('btn-play-again').onclick = () => $('btn-reset').click();
$('btn-see-board').onclick = () => $('gameover-modal').classList.add('hidden');
$('btn-sound').onclick = () => { audio.soundEnabled = !audio.soundEnabled; $('btn-sound').classList.toggle('active', audio.soundEnabled); $('btn-sound').innerText = audio.soundEnabled ? 'Effetti attivi' : 'Effetti spenti'; };
$('btn-ambient').onclick = () => { const a = audio.toggleAmbient(); $('btn-ambient').classList.toggle('active', a); };
const CAMS = ['white', 'black', 'top', 'nave', 'apse', 'cinematic'];
CAMS.forEach(k => $(`cam-${k}`).onclick = () => { setActive(CAMS.map(x => `cam-${x}`), `cam-${k}`);
  if (k === 'cinematic') { isCinemaMode = true; setCameraPreset(game.turn() === 'w' ? 'white' : 'black'); } else { isCinemaMode = false; setCameraPreset(k); }
  if (isNarrow()) closeMenu(); });
document.querySelectorAll('.promo-btn').forEach(b => b.onclick = e => { if (!pendingPromotionMove) return; const m = pendingPromotionMove; m.promotion = e.currentTarget.dataset.piece; pendingPromotionMove = null; $('promotion-modal').classList.add('hidden'); executeMove(m); });
let resizeT = null;
function onResize() { if (!innerWidth || !innerHeight) return; camera.aspect = innerWidth / innerHeight; const pr = Math.max(0.75, targetPixelRatio()); renderer.setPixelRatio(pr); renderer.setSize(innerWidth, innerHeight); if (composer) { composer.setPixelRatio(pr); composer.setSize(innerWidth, innerHeight); } applyViewOffset();
  clearTimeout(resizeT); resizeT = setTimeout(() => { applyViewOffset(); if (BOARD_PRESETS[currentPreset]) setCameraPreset(currentPreset); }, 250); }
addEventListener('resize', onResize);
if (window.visualViewport) visualViewport.addEventListener('resize', onResize);

// =====================================================================
// AVVIO
// =====================================================================
if (!setupPost()) document.getElementById('vignette').classList.remove('hidden');
bake(staticG);
applyTheme(); applySettings(); renderBoardState();
setTimeout(() => { buildEnv(); $('loader').style.opacity = 0; setCameraPreset('white'); setTimeout(() => $('loader').remove(), 1700); }, 300);

// =====================================================================
// LOOP DI ANIMAZIONE
// =====================================================================
const reduceMotion = matchMedia('(prefers-reduced-motion: reduce)').matches;
const clock = new THREE.Clock(), tmpV = new THREE.Vector3(), altarSmoke = new THREE.Vector3(0, SY + 1.4, -23.7);
function animateLoop() {
  requestAnimationFrame(animateLoop);
  const dt = Math.min(clock.getDelta(), 0.05), t = clock.elapsedTime, k = reduceMotion ? 0.3 : 1, cm = isFinite(S.candles) ? S.candles : 1;
  controls.update(); clampCamera();

  flames.forEach(f => { const fl = 0.86 + Math.sin(t * 7.3 + f.phase) * 0.06 + Math.sin(t * 13.1 + f.phase * 1.7) * 0.04 + Math.sin(t * 2.1 + f.phase * 0.6) * 0.03;
    f.fg.scale.set(1, fl, 1); f.fg.rotation.z = Math.sin(t * 3 + f.phase) * 0.08; f.glow.scale.setScalar(f.base * fl * Math.max(cm, 0.05)); f.glow.material.opacity = 0.75 * Math.min(cm, 1.2); f.fg.visible = cm > 0.02; });
  flickerLights.forEach(f => { f.light.intensity = f.base * (isFinite(cm) ? cm : 1) * (0.9 + Math.sin(t * 5.7 + f.phase) * 0.05 + Math.sin(t * 11.3 + f.phase * 1.3) * 0.03); });
  shafts.forEach(s => { s.mesh.material.opacity = s.base * (0.75 + Math.sin(t * 0.35 + s.phase) * 0.25); });
  gargoyleEyes.forEach((e, i) => { e.material.opacity = 0.35 + Math.max(0, Math.sin(t * 0.6 + i * 1.3)) * 0.5; });

  if (figuresG.visible) {
    CLOTH_T.value = t; updateArchmages(t, dt);
    monks.forEach((m, i) => {
      if (m.armR) m.armR.p.rotation.x = (m.censer ? -0.55 : -0.45) + Math.sin(t * 3.2 + i) * 0.08;
      m.u = (m.u + m.speed * dt * k * 0.6) % 1; const p = procPath.getPointAt(m.u), tg = procPath.getTangentAt(m.u);
      const step = t * 3.2 + i; m.g.position.set(p.x, FY + Math.abs(Math.sin(step)) * 0.025, p.z); m.g.rotation.y = Math.atan2(tg.x, tg.z);
      m.robe.rotation.y = Math.sin(step) * 0.05; m.g.rotation.z = Math.sin(step) * 0.015; m.head.rotation.x = 0.15 + Math.sin(t * 0.3 + i) * 0.05;
      if (m.censer) { m.censer.pivot.rotation.x = Math.sin(t * 1.9) * 0.75 * k; m.censer.bowl.getWorldPosition(censerSmokeSrc); }
      if (m.lantern) { m.lantern.rotation.x = Math.sin(step) * 0.12; m.lantern.rotation.z = Math.cos(step * 0.5) * 0.06; }
    });
    const lift = (Math.sin(t * 0.35) + 1) / 2;
    bishop.arms.forEach((a, i) => { a.rotation.x = -(0.35 + lift * 1.7) * (k > 0.5 ? 1 : 0.5); a.rotation.z = (i ? -1 : 1) * (0.25 + lift * 0.15); });
    bishop.head.rotation.x = -lift * 0.25;
    const sing = audio.ambientActive ? 1 : 0.35;
    choir.forEach(c => { c.head.rotation.x = 0.1 + Math.sin(t * 1.4 + c.phase) * 0.08 * sing; c.g.rotation.z = Math.sin(t * 0.7 + c.phase) * 0.025 * sing; });
    penitents.forEach(p => { p.g.rotation.x = -0.25 - (Math.sin(t * 0.5 + p.phase) + 1) * 0.12 * k; p.head.rotation.x = 0.35; });
  }
  if (creaturesG.visible) {
    bats.forEach(b => { b.a += b.sp * k; b.g.position.set(Math.cos(b.a) * b.rx, b.y + Math.sin(t * 1.5 + b.ph) * 0.7, Math.sin(b.a) * b.rz);
      b.g.rotation.y = Math.atan2(-Math.sin(b.a) * b.rx, Math.cos(b.a) * b.rz); const f = Math.sin(t * b.fl + b.ph) * 0.9; b.lp.rotation.z = f; b.rp.rotation.z = -f; });
    crows.forEach(c => { const s = Math.sin(t * 0.4 + c.ph); c.h.rotation.y = s > 0.5 ? 0.9 : s < -0.5 ? -0.9 : 0; c.h.rotation.x = Math.sin(t * 2.3 + c.ph) > 0.97 ? 0.5 : 0; });
  }
  if (dust.visible) {
    const pa = dust.geometry.attributes.position.array;
    for (let i = 0; i < dustCount * 3; i += 3) { pa[i + 1] -= 0.0035 * k; pa[i] += Math.sin(t * 0.3 + i) * 0.0015; if (pa[i + 1] < FY) pa[i + 1] = FY + 32; }
    dust.geometry.attributes.position.needsUpdate = true;
    smoke.forEach(s => { s.t += dt * 0.09 * k; if (s.t > 1) s.t = 0; const src = s.src && figuresG.visible ? censerSmokeSrc : altarSmoke;
      if (s.t < 0.02) s.origin = src.clone(); const o = s.origin || src;
      s.s.position.set(o.x + Math.sin(s.t * 5 + s.dx * 6) * s.t * 0.9 + s.dx * 0.2, o.y + s.t * 6, o.z + Math.cos(s.t * 3 + s.dx) * s.t * 0.6);
      s.s.scale.setScalar(0.3 + s.t * 2.6); s.s.material.opacity = 0.22 * Math.sin(s.t * Math.PI); });
  }
  // --- oggetti aggiuntivi ---
  updateHourglass(t, dt);
  const organOn = audio.ambientActive ? 1 : 0;
  organPipes.forEach(o => { const target = organOn * (0.25 + 0.25 * Math.max(0, Math.sin(t * 1.3 + o.ph))); o.m.emissiveIntensity += (target - o.m.emissiveIntensity) * Math.min(1, dt * 3); });
  if (figuresG.visible) swingLanterns.forEach(l => { l.pivot.rotation.z = Math.sin(t * 0.9 + l.ph) * l.amp * k; l.pivot.rotation.x = Math.cos(t * 0.7 + l.ph) * l.amp * 0.5 * k; });
  if (mistG.visible) mistLayers.forEach(m => { m.tex.offset.x += m.sp * m.dir * dt * k; m.tex.offset.y += m.sp * 0.6 * dt * k; });
  if (patchG.visible) patches.forEach(p => { p.m.material.opacity = (0.08 + 0.05 * Math.sin(t * 0.25 + p.ph)) * (themeBase ? themeBase.glass : 0.7); });
  if (creaturesG.visible) updateRats(t, dt);
  // pezzo selezionato che levita
  Object.values(pieceObjects).forEach(m => { if (m.userData.moving || (toppling && toppling.mesh === m)) return;
    const target = m === selectedMesh ? 0.36 + Math.sin(t * 3) * 0.04 : 0.08; m.position.y += (target - m.position.y) * Math.min(1, dt * 10);
    if (m === selectedMesh) m.rotation.y += dt * 0.8 * k; else if (m.userData.spun) { m.rotation.y = m.userData.baseRot; } });
  if (selectedMesh && !selectedMesh.userData.spun) { selectedMesh.userData.spun = true; selectedMesh.userData.baseRot = selectedMesh.rotation.y; }
  if (selRing.visible) { const s = 1 + Math.sin(t * 4) * 0.06; selRing.scale.set(s, s, s); selRing.material.opacity = 0.5 + Math.sin(t * 4) * 0.2; }
  // re sotto scacco: luce rossa pulsante
  if (checkSquare && tiles[checkSquare]) { const tt = tiles[checkSquare]; checkLight.position.set(tt.position.x, 0.9, tt.position.z); checkLight.intensity = 1.6 + Math.sin(t * 6) * 0.8; } else checkLight.intensity = 0;
  // re che cade allo scacco matto
  if (toppling) { toppling.t = Math.min(1, toppling.t + dt / 1.1); const e = toppling.t < 1 ? 1 - Math.pow(1 - toppling.t, 3) : 1; toppling.mesh.rotation.z = toppling.dir * e * Math.PI / 2 * 0.98; toppling.mesh.position.y = 0.08 + Math.sin(e * Math.PI) * 0.15 + e * 0.12; }
  // pezzi catturati che si dissolvono in braci
  for (let i = dying.length - 1; i >= 0; i--) { const d = dying[i]; d.t += dt / 1.2; d.mesh.position.y += dt * 1.1; d.mesh.rotation.y += d.spin * dt; d.mesh.scale.setScalar(Math.max(0.01, 1 - d.t * 0.6)); d.mats.forEach(m => m.opacity = Math.max(0, 1 - d.t));
    if (d.t >= 1) { scene.remove(d.mesh); d.mats.forEach(m => m.dispose()); dying.splice(i, 1); } }
  // temporale
  if (themeBase) {
    let f = 0;
    if (S.storm && !reduceMotion) { if (flashT < 0) { nextFlash -= dt; if (nextFlash <= 0) { flashT = 0; const pw = 0.6 + Math.random() * 0.4; setTimeout(() => audio.thunder(pw), 700 + Math.random() * 1800); } }
      else { flashT += dt; f = flashCurve(flashT); if (flashT > 0.6) { flashT = -1; nextFlash = 18 + Math.random() * 30; } } }
    moon.intensity = themeBase.moon[1] + f * 3.2; ambientLight.intensity = themeBase.amb[1] + f * 0.5;
    if (f > 0.01 || moon.userData.flashed) { glassMats.forEach(m => m.color.setScalar(themeBase.glass * (1 + f * 2.5))); moon.userData.flashed = f > 0.01; }
  }

  updateShards(dt);
  for (let i = sparks.length - 1; i >= 0; i--) { const p = sparks[i]; p.s.position.add(p.v); p.v.y -= 0.0025; p.life -= 0.03; p.s.material.opacity = p.life; if (p.life <= 0) { scene.remove(p.s); p.s.material.dispose(); sparks.splice(i, 1); } }
  camShake = Math.max(0, camShake - dt * 0.6); const shk = new THREE.Vector3((Math.random() - 0.5) * camShake, (Math.random() - 0.5) * camShake, (Math.random() - 0.5) * camShake); camera.position.add(shk);
  if (composer) { finalPass.uniforms.time.value = t % 100; composer.render(dt); postCheck(); } else renderer.render(scene, camera);
  camera.position.sub(shk);
}
let postFrames = 0;
function postCheck() {
  if (++postFrames !== 20) return;
  try { const gl = renderer.getContext(), w = gl.drawingBufferWidth, h = gl.drawingBufferHeight, px = new Uint8Array(4); let sum = 0;
    for (let i = 1; i <= 3; i++) for (let j = 1; j <= 3; j++) { gl.readPixels(Math.floor(w * i / 4), Math.floor(h * j / 4), 1, 1, gl.RGBA, gl.UNSIGNED_BYTE, px); sum += px[0] + px[1] + px[2]; }
    if (sum === 0) { composer = null; finalPass = null; renderer.toneMapping = THREE.ACESFilmicToneMapping; document.getElementById('vignette').classList.remove('hidden'); scene.traverse(o => { if (o.material) [].concat(o.material).forEach(m => m.needsUpdate = true); }); }
  } catch (e) {}
}
animateLoop();
</script>
</body>
</html>
