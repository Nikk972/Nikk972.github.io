# Il mio codice di prova

Ecco un esempio di codice HTML che ho scritto:

```<!DOCTYPE html>
<html lang="it" class="h-full">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Sanctum Chess – Cattedrale Gotica</title>
<script src="https://cdn.tailwindcss.com/3.4.17"></script>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;600;700;900&family=Cormorant+Garamond:ital,wght@0,400;0,600;1,400&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/chess.js/0.10.3/chess.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
<style>
:root{box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px);}
html,body{height:100%}
body{font-family:'Cormorant Garamond',Georgia,serif;user-select:none;overflow:hidden;background:#020203;color:#cfc3ae;touch-action:none;margin:0}
h1,h2,.cinzel,button{font-family:'Cinzel',Georgia,serif}
.gothic-border{border:1px solid #4a3a26;box-shadow:0 0 24px rgba(0,0,0,.95),inset 0 0 12px rgba(90,70,40,.15)}
.gothic-card{background:rgba(6,5,8,.86);backdrop-filter:blur(8px)}
.gothic-btn{background:linear-gradient(160deg,#120d09 0%,#21180f 100%);border:1px solid #5c4a30;color:#cfc3ae;transition:all .2s ease}
.gothic-btn:hover{border-color:#a8854f;color:#fff;box-shadow:0 0 10px rgba(168,133,79,.3)}
.gothic-btn.active{background:linear-gradient(160deg,#3a2a16 0%,#5c4426 100%);color:#fff;border-color:#c9a35f;box-shadow:0 0 12px rgba(201,163,95,.35)}
.gothic-btn:focus-visible,select:focus-visible,input:focus-visible{outline:2px solid #c9a35f;outline-offset:2px}
@keyframes pulse-gothic{0%,100%{box-shadow:0 0 15px rgba(180,30,30,.4)}50%{box-shadow:0 0 30px rgba(200,40,40,.8)}}
.in-check-alert{animation:pulse-gothic 1.5s infinite;border-color:#a32020!important}
::-webkit-scrollbar{width:4px}::-webkit-scrollbar-thumb{background:#4a3a26;border-radius:2px}
.set-row{display:flex;justify-content:space-between;align-items:center;gap:1rem;padding:.35rem 0;border-bottom:1px solid rgba(92,74,48,.25)}
.set-row span{font-size:1rem}
.set-row input[type=range]{width:9.5rem;accent-color:#a8854f}
.set-row input[type=checkbox]{width:1.1rem;height:1.1rem;accent-color:#a8854f;cursor:pointer}
.set-row select{background:#120d09;border:1px solid #5c4a30;color:#cfc3ae;border-radius:.3rem;padding:.2rem .4rem;font-family:'Cinzel',serif;font-size:.75rem}
.tab-btn{font-size:.72rem;padding:.45rem .6rem;border-bottom:2px solid transparent;color:#9c8b70}
.tab-btn.active{color:#e8d5b0;border-color:#a8854f}
#loader{transition:opacity 1.6s ease}
</style>
</head>
<body class="w-full h-full m-0 p-0 overflow-hidden relative">

<div id="canvas-container" class="absolute inset-0 z-0 cursor-grab active:cursor-grabbing"></div>

<div id="loader" class="fixed inset-0 z-[60] bg-black flex flex-col items-center justify-center pointer-events-none">
  <div class="text-5xl mb-4 text-amber-700/80">✠</div>
  <div class="cinzel text-amber-600/80 tracking-[0.35em] text-xs">Si aprono i portali</div>
</div>

<div class="relative z-10 w-full h-full pointer-events-none flex flex-col justify-between p-2 sm:p-4">
  <div class="flex flex-col sm:flex-row justify-between items-stretch sm:items-start gap-2.5 w-full">
    <div id="status-card" class="gothic-card gothic-border p-3 rounded-md pointer-events-auto sm:w-72 transition-all">
      <div class="flex justify-between items-center">
        <h1 class="text-base sm:text-lg font-black text-amber-600/90 tracking-wider">✠ Sanctum Chess</h1>
        <button id="btn-mobile-menu" class="sm:hidden gothic-btn px-2 py-1 rounded text-xs">☰</button>
      </div>
      <div class="mt-2 pt-2 border-t border-amber-900/40 flex justify-between items-center text-base">
        <span>Turno: <strong id="game-status" class="text-amber-500">Bianco</strong></span>
        <span>Stato: <strong id="check-status" class="text-emerald-500/80">Sicuro</strong></span>
      </div>
    </div>

    <div id="control-panel" class="gothic-card gothic-border p-2.5 sm:p-3 rounded-md pointer-events-auto flex flex-wrap items-center gap-2.5">
      <div class="flex flex-col gap-1">
        <span class="text-xs text-amber-600/80">Modalità</span>
        <div class="flex gap-1 text-xs">
          <button id="btn-mode-pvp" class="gothic-btn px-2.5 py-1 rounded active">PvP</button>
          <button id="btn-mode-ai" class="gothic-btn px-2.5 py-1 rounded">vs IA</button>
        </div>
      </div>
      <div id="ai-difficulty-container" class="hidden flex-col gap-1">
        <span class="text-xs text-amber-600/80">Difficoltà</span>
        <div class="flex gap-1 text-xs">
          <button id="ai-easy" class="gothic-btn px-2 py-1 rounded">Accolito</button>
          <button id="ai-med" class="gothic-btn px-2 py-1 rounded active">Vescovo</button>
          <button id="ai-hard" class="gothic-btn px-2 py-1 rounded">Gran Maestro</button>
        </div>
      </div>
      <div class="flex flex-col gap-1">
        <span class="text-xs text-amber-600/80">Tempo</span>
        <div class="flex gap-1 text-xs">
          <button id="timer-off" class="gothic-btn px-2 py-1 rounded active">Off</button>
          <button id="timer-3m" class="gothic-btn px-2 py-1 rounded">3m</button>
          <button id="timer-5m" class="gothic-btn px-2 py-1 rounded">5m</button>
        </div>
      </div>
      <div id="timer-display-container" class="hidden items-center gap-2 text-sm bg-black/50 px-2.5 py-1 rounded border border-amber-900/40">
        <span>B <strong id="timer-white" class="text-amber-500">03:00</strong></span><span class="text-amber-900">|</span><span>N <strong id="timer-black" class="text-amber-500">03:00</strong></span>
      </div>
      <div class="flex items-center gap-1.5 ml-auto">
        <button id="btn-undo" class="gothic-btn px-2 py-1.5 rounded text-xs" title="Annulla mossa">↶ <span class="hidden md:inline">Indietro</span></button>
        <button id="btn-hint" class="gothic-btn px-2 py-1.5 rounded text-xs" title="Suggerimento">✦ <span class="hidden md:inline">Aiuto</span></button>
        <button id="btn-settings-modal" class="gothic-btn px-2 py-1.5 rounded text-xs" title="Impostazioni">⚙ <span class="hidden md:inline">Impostazioni</span></button>
        <button id="btn-reset" class="gothic-btn px-2 py-1.5 rounded text-xs" title="Nuova partita">⟳</button>
      </div>
    </div>
  </div>

  <div class="absolute right-4 top-28 bottom-24 w-60 pointer-events-none hidden xl:flex flex-col gap-3 z-20">
    <div class="gothic-card gothic-border p-3 rounded-md pointer-events-auto">
      <div class="cinzel text-xs text-amber-600/90 mb-1 border-b border-amber-900/40 pb-1">Pezzi caduti</div>
      <div class="flex flex-col gap-1.5 mt-2">
        <div class="flex gap-1 min-h-[22px] text-lg text-stone-200 flex-wrap" id="graveyard-white"></div>
        <div class="flex gap-1 min-h-[22px] text-lg text-stone-500 flex-wrap" id="graveyard-black"></div>
      </div>
    </div>
    <div class="gothic-card gothic-border p-3 rounded-md flex flex-col flex-1 overflow-hidden pointer-events-auto">
      <div class="cinzel text-xs text-amber-600/90 mb-1 border-b border-amber-900/40 pb-1 shrink-0">Annali della cattedrale</div>
      <div id="move-log" class="overflow-y-auto flex-1 text-sm space-y-0.5 my-1 pr-1"></div>
    </div>
  </div>

  <div class="flex flex-wrap justify-between items-center gap-2 w-full">
    <div class="gothic-card gothic-border p-1.5 sm:p-2 rounded-md pointer-events-auto flex items-center gap-2">
      <button id="btn-sound" class="gothic-btn p-2 rounded text-sm" title="Effetti sonori"><span id="sound-icon">🔊</span></button>
      <button id="btn-ambient" class="gothic-btn p-2 rounded text-sm" title="Organo e coro"><span id="ambient-icon">🎵</span></button>
    </div>
    <div class="gothic-card gothic-border p-1.5 sm:p-2 rounded-md pointer-events-auto flex flex-wrap items-center gap-1 text-xs">
      <button id="cam-white" class="gothic-btn px-2 py-1.5 rounded active">Bianco</button>
      <button id="cam-black" class="gothic-btn px-2 py-1.5 rounded">Nero</button>
      <button id="cam-top" class="gothic-btn px-2 py-1.5 rounded">Alto</button>
      <button id="cam-nave" class="gothic-btn px-2 py-1.5 rounded">Navata</button>
      <button id="cam-apse" class="gothic-btn px-2 py-1.5 rounded">Altare</button>
      <button id="cam-cinematic" class="gothic-btn px-2 py-1.5 rounded">Cinema</button>
    </div>
  </div>
</div>

<div id="promotion-modal" class="fixed inset-0 z-50 bg-black/85 hidden flex items-center justify-center p-4">
  <div class="gothic-card gothic-border p-6 rounded-lg text-center max-w-md w-full pointer-events-auto">
    <h2 class="text-xl text-amber-500 mb-2">Ascesa del pedone</h2>
    <p class="text-base mb-6">Scegli in cosa promuovere il pedone</p>
    <div class="grid grid-cols-4 gap-3">
      <button data-piece="q" class="promo-btn gothic-btn p-4 rounded flex flex-col items-center gap-2"><span class="text-3xl">♛</span><span class="text-xs">Regina</span></button>
      <button data-piece="r" class="promo-btn gothic-btn p-4 rounded flex flex-col items-center gap-2"><span class="text-3xl">♜</span><span class="text-xs">Torre</span></button>
      <button data-piece="b" class="promo-btn gothic-btn p-4 rounded flex flex-col items-center gap-2"><span class="text-3xl">♝</span><span class="text-xs">Alfiere</span></button>
      <button data-piece="n" class="promo-btn gothic-btn p-4 rounded flex flex-col items-center gap-2"><span class="text-3xl">♞</span><span class="text-xs">Cavallo</span></button>
    </div>
  </div>
</div>

<div id="settings-modal" class="fixed inset-0 z-50 bg-black/75 hidden flex items-center justify-center p-4">
  <div class="gothic-card gothic-border rounded-lg max-w-lg w-full pointer-events-auto flex flex-col max-h-[88vh]">
    <div class="flex justify-between items-center px-5 pt-4 pb-2">
      <h2 class="text-lg text-amber-500">Impostazioni</h2>
      <button id="close-settings" class="text-amber-500 hover:text-white text-xl" aria-label="Chiudi">✕</button>
    </div>
    <div class="flex px-4 border-b border-amber-900/40">
      <button class="tab-btn active" data-tab="gfx">Grafica</button>
      <button class="tab-btn" data-tab="env">Ambiente</button>
      <button class="tab-btn" data-tab="audio">Audio</button>
      <button class="tab-btn" data-tab="game">Gioco</button>
    </div>
    <div class="px-5 py-3 overflow-y-auto">
      <div data-panel="gfx">
        <label class="set-row"><span>Qualità</span><select data-key="quality"><option value="low">Bassa</option><option value="med">Media</option><option value="high">Alta</option></select></label>
        <label class="set-row"><span>Ombre</span><select data-key="shadows"><option value="off">Disattivate</option><option value="soft">Morbide</option><option value="sharp">Nitide</option></select></label>
        <label class="set-row"><span>Riflessi su oro e marmo</span><input type="checkbox" data-key="reflections"></label>
        <label class="set-row"><span>Luminosità</span><input type="range" data-key="exposure" min="0.4" max="1.8" step="0.05"></label>
        <label class="set-row"><span>Campo visivo</span><input type="range" data-key="fov" min="30" max="75" step="1"></label>
        <label class="set-row"><span>Rotazione automatica</span><input type="checkbox" data-key="autorotate"></label>
      </div>
      <div data-panel="env" class="hidden">
        <label class="set-row"><span>Atmosfera</span><select data-key="theme"><option value="sanctum">Candele del Sanctum</option><option value="moonlight">Notte di luna</option><option value="vespers">Vespri al tramonto</option><option value="crypt">Cripta sanguigna</option></select></label>
        <label class="set-row"><span>Nebbia e incenso</span><input type="range" data-key="fog" min="0" max="0.05" step="0.002"></label>
        <label class="set-row"><span>Intensità candele</span><input type="range" data-key="candles" min="0" max="2" step="0.05"></label>
        <label class="set-row"><span>Fasci di luce dalle vetrate</span><input type="checkbox" data-key="shafts"></label>
        <label class="set-row"><span>Pulviscolo e fumo</span><input type="checkbox" data-key="dust"></label>
        <label class="set-row"><span>Monaci, coro e vescovo</span><input type="checkbox" data-key="figures"></label>
        <label class="set-row"><span>Spettro, pipistrelli e corvi</span><input type="checkbox" data-key="creatures"></label>
      </div>
      <div data-panel="audio" class="hidden">
        <label class="set-row"><span>Volume effetti</span><input type="range" data-key="sfx" min="0" max="1" step="0.05"></label>
        <label class="set-row"><span>Volume organo e coro</span><input type="range" data-key="music" min="0" max="1" step="0.05"></label>
        <label class="set-row"><span>Riverbero della navata</span><input type="range" data-key="reverb" min="0" max="1" step="0.05"></label>
        <label class="set-row"><span>Campana a fine partita</span><input type="checkbox" data-key="bell"></label>
      </div>
      <div data-panel="game" class="hidden">
        <label class="set-row"><span>Scacchiera</span><select data-key="boardStyle"><option value="marble">Marmo bianco e nero</option><option value="obsidian">Pietra e ossidiana</option><option value="bone">Osso ed ebano</option></select></label>
        <label class="set-row"><span>Pezzi</span><select data-key="pieceSet"><option value="ivory">Avorio ed ebano</option><option value="metal">Argento e bronzo</option><option value="bone">Osso e ossidiana</option></select></label>
        <label class="set-row"><span>Evidenzia mosse legali</span><input type="checkbox" data-key="highlights"></label>
        <label class="set-row"><span>Mostra ultima mossa</span><input type="checkbox" data-key="lastmove"></label>
        <label class="set-row"><span>Velocità dei pezzi</span><input type="range" data-key="animSpeed" min="0.5" max="2.5" step="0.1"></label>
      </div>
    </div>
    <div class="px-5 pb-4 pt-2 flex justify-end">
      <button id="btn-defaults" class="gothic-btn px-3 py-1.5 rounded text-xs">Ripristina predefiniti</button>
    </div>
  </div>
</div>

<div id="gameover-modal" class="fixed inset-0 z-50 bg-black/90 hidden flex items-center justify-center p-4">
  <div class="gothic-card gothic-border p-8 rounded-lg text-center max-w-sm w-full pointer-events-auto">
    <div class="text-amber-600 text-4xl mb-3">✠</div>
    <h2 id="gameover-title" class="text-2xl text-amber-500 mb-1">Scacco matto!</h2>
    <p id="gameover-desc" class="text-lg mb-6">Il santuario ha decretato il vincitore.</p>
    <button id="btn-play-again" class="gothic-btn w-full py-3 rounded font-bold text-sm tracking-wider">Gioca di nuovo</button>
  </div>
</div>

<script>
// =====================================================================
// IMPOSTAZIONI
// =====================================================================
const DEFAULTS = { quality:'high', shadows:'soft', reflections:true, exposure:1.0, fov:45, autorotate:false,
  theme:'sanctum', fog:0.024, candles:1, shafts:true, dust:true, figures:true, creatures:true,
  sfx:0.7, music:0.5, reverb:0.55, bell:true,
  boardStyle:'marble', pieceSet:'ivory', highlights:true, lastmove:true, animSpeed:1 };
let S = { ...DEFAULTS };
try { const saved = JSON.parse(localStorage.getItem('sanctum-settings') || '{}'); S = { ...DEFAULTS, ...saved }; } catch (e) {}
function saveSettings() { try { localStorage.setItem('sanctum-settings', JSON.stringify(S)); } catch (e) {} }

// =====================================================================
// RENDERER, SCENA, CAMERA
// =====================================================================
const container = document.getElementById('canvas-container');
const scene = new THREE.Scene();
scene.background = new THREE.Color(0x040306);
scene.fog = new THREE.FogExp2(0x050407, S.fog);
const camera = new THREE.PerspectiveCamera(S.fov, innerWidth / innerHeight, 0.1, 400);
const renderer = new THREE.WebGLRenderer({ antialias: true, powerPreference: 'high-performance' });
renderer.setSize(innerWidth, innerHeight);
renderer.outputEncoding = THREE.sRGBEncoding;
renderer.toneMapping = THREE.ACESFilmicToneMapping;
renderer.shadowMap.enabled = true;
renderer.shadowMap.type = THREE.PCFSoftShadowMap;
container.appendChild(renderer.domElement);

const controls = new THREE.OrbitControls(camera, renderer.domElement);
controls.enableDamping = true; controls.dampingFactor = 0.06;
controls.maxPolarAngle = Math.PI / 2 - 0.04; controls.minDistance = 3.5; controls.maxDistance = 46;
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
function setCameraPreset(key, instant = false) {
  const p = cameraPresets[key]; if (!p) return;
  if (instant) { camera.position.copy(p.pos); controls.target.copy(p.target); controls.update(); return; }
  const sp = camera.position.clone(), st = controls.target.clone(); let pr = 0;
  (function anim() { pr += 0.022; const t = pr < 0.5 ? 4 * pr ** 3 : 1 - Math.pow(-2 * pr + 2, 3) / 2;
    if (pr <= 1) { camera.position.lerpVectors(sp, p.pos, t); controls.target.lerpVectors(st, p.target, t); requestAnimationFrame(anim); }
    else { camera.position.copy(p.pos); controls.target.copy(p.target); } })();
}
camera.position.set(0, 9, 27); controls.target.set(0, 6, -6);

// =====================================================================
// AUDIO: riverbero da cattedrale, organo, coro, campana
// =====================================================================
class GothicAudio {
  constructor() { this.ctx = null; this.soundEnabled = true; this.ambientActive = false; this.timer = null; }
  init() {
    if (this.ctx) return; const c = this.ctx = new (window.AudioContext || window.webkitAudioContext)();
    this.master = c.createGain(); this.master.connect(c.destination);
    this.reverb = c.createConvolver(); const len = c.sampleRate * 5, buf = c.createBuffer(2, len, c.sampleRate);
    for (let ch = 0; ch < 2; ch++) { const d = buf.getChannelData(ch); for (let i = 0; i < len; i++) d[i] = (Math.random() * 2 - 1) * Math.pow(1 - i / len, 2.6); }
    this.reverb.buffer = buf; this.wet = c.createGain(); this.reverb.connect(this.wet); this.wet.connect(this.master);
    this.sfx = c.createGain(); this.sfx.connect(this.master); this.sfx.connect(this.reverb);
    this.music = c.createGain(); this.music.connect(this.master); this.music.connect(this.reverb);
    this.setVolumes();
  }
  setVolumes() { if (!this.ctx) return; this.sfx.gain.value = S.sfx; this.music.gain.value = S.music * 0.8; this.wet.gain.value = S.reverb * 1.4; }
  tone(type, f1, f2, vol, dur, delay = 0) {
    const t0 = this.ctx.currentTime + delay, o = this.ctx.createOscillator(), g = this.ctx.createGain();
    o.type = type; o.frequency.setValueAtTime(f1, t0); if (f2) o.frequency.exponentialRampToValueAtTime(f2, t0 + dur);
    g.gain.setValueAtTime(vol, t0); g.gain.exponentialRampToValueAtTime(0.0008, t0 + dur);
    o.connect(g); g.connect(this.sfx); o.start(t0); o.stop(t0 + dur + 0.05);
  }
  playMove() { if (!this.soundEnabled) return; this.init(); this.tone('triangle', 150, 60, 0.35, 0.2); this.tone('sine', 90, 40, 0.3, 0.25); }
  playCapture() { if (!this.soundEnabled) return; this.init(); this.tone('sawtooth', 110, 30, 0.3, 0.35); this.tone('square', 70, 25, 0.15, 0.4); }
  playCheck() { if (!this.soundEnabled) return; this.init(); [392, 466.16, 587.33].forEach((f, i) => this.tone('sine', f, null, 0.16, 1.2, i * 0.09)); }
  bell() {
    if (!this.soundEnabled || !S.bell) return; this.init();
    [0.5, 1, 1.19, 1.5, 2, 2.52, 3.01].forEach((m, i) => this.tone('sine', 196 * m, null, 0.22 / (i + 1), 6 - i * 0.5));
    setTimeout(() => [0.5, 1, 1.19, 1.5, 2].forEach((m, i) => this.tone('sine', 196 * m, null, 0.18 / (i + 1), 5)), 2200);
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
    this.init(); this.ambientActive = !this.ambientActive;
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
function updateTimerUI() { const f = s => `${String(Math.floor(s / 60)).padStart(2, '0')}:${String(s % 60).padStart(2, '0')}`; document.getElementById('timer-white').innerText = f(timeWhite); document.getElementById('timer-black').innerText = f(timeBlack); }
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
const ashlar = texSet(512, (g, hg, s) => {
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
// Pavimento: lastre di pietra e marmo
const floorSet = texSet(512, (g, hg, s) => {
  const n = 4, t = s / n;
  for (let y = 0; y < n; y++) for (let x = 0; x < n; x++) {
    const dark = (x + y) % 2, l = dark ? 9 + Math.random() * 3 : 30 + Math.random() * 6;
    g.fillStyle = `hsl(${dark ? 260 : 38},${dark ? 6 : 9}%,${l}%)`; g.fillRect(x * t, y * t, t, t);
    hg.fillStyle = '#b0b0b0'; hg.fillRect(x * t, y * t, t, t);
    for (let v = 0; v < 5; v++) { g.strokeStyle = `rgba(255,250,240,${dark ? 0.05 : 0.09})`; g.lineWidth = 0.6 + Math.random(); g.beginPath();
      g.moveTo(x * t + Math.random() * t, y * t); g.bezierCurveTo(x * t + Math.random() * t, y * t + t * 0.4, x * t + Math.random() * t, y * t + t * 0.6, x * t + Math.random() * t, y * t + t); g.stroke(); }
    speckle(g, hg, x * t, y * t, t, t, 120, true);
    if (Math.random() > 0.7) { hg.strokeStyle = 'rgba(0,0,0,0.6)'; hg.beginPath(); hg.moveTo(x * t + Math.random() * t, y * t + Math.random() * t); hg.lineTo(x * t + Math.random() * t, y * t + Math.random() * t); hg.stroke(); g.strokeStyle = 'rgba(0,0,0,0.4)'; g.stroke(); }
  }
  g.strokeStyle = '#060506'; hg.strokeStyle = '#000'; g.lineWidth = hg.lineWidth = 3;
  for (let i = 0; i <= n; i++) { [g, hg].forEach(c => { c.beginPath(); c.moveTo(i * t, 0); c.lineTo(i * t, s); c.stroke(); c.beginPath(); c.moveTo(0, i * t); c.lineTo(s, i * t); c.stroke(); }); }
}, 4, 0.25);
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
const marbleW = marbleTex('#e8e1d2', '#6a6158'), marbleB = marbleTex('#141216', '#8a7a62');
// Vetrate
function lancetTex(seed) {
  const pal = [['#7a0c1c', '#10307a', '#a8700f', '#0c5230', '#3e1460'], ['#123070', '#8a1020', '#b0841a', '#4a1666', '#0e4a3a'], ['#5a0e60', '#a01818', '#1a3a8a', '#9a7a14', '#105030']][seed % 3];
  const c = mkCanvas(160, 400), g = c.getContext('2d'), w = 160, h = 400;
  for (let y = 0; y < h; y += 14) for (let x = 0; x < w; x += 14) { g.fillStyle = pal[(Math.random() * 5) | 0]; g.globalAlpha = 0.6 + Math.random() * 0.4; g.fillRect(x, y, 14, 14); }
  g.globalAlpha = 1;
  for (let m = 0; m < 3; m++) { const cy = 90 + m * 115; g.fillStyle = pal[(m + seed) % 5]; g.beginPath(); g.arc(w / 2, cy, 46, 0, 7); g.fill();
    g.fillStyle = '#c9a050'; g.beginPath(); g.arc(w / 2, cy - 16, 13, 0, 7); g.fill(); g.fillStyle = '#d8b890'; g.beginPath(); g.arc(w / 2, cy - 16, 7, 0, 7); g.fill();
    g.fillStyle = pal[(m + seed + 2) % 5]; g.beginPath(); g.moveTo(w / 2 - 20, cy + 36); g.lineTo(w / 2, cy - 8); g.lineTo(w / 2 + 20, cy + 36); g.fill();
    g.strokeStyle = '#0b0806'; g.lineWidth = 5; g.beginPath(); g.arc(w / 2, cy, 46, 0, 7); g.stroke(); }
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
  const s = new THREE.Sprite(new THREE.SpriteMaterial({ map: glowTex, color, transparent: true, opacity, blending: THREE.AdditiveBlending, depthWrite: false, fog: true }));
  s.scale.set(size, size, 1); return s;
}

// =====================================================================
// MATERIALI
// =====================================================================
const wallMat = new THREE.MeshStandardMaterial({ map: ashlar.map, normalMap: ashlar.normalMap, color: 0xa29a92, roughness: 0.92, side: THREE.DoubleSide });
const stoneMat = new THREE.MeshStandardMaterial({ map: dressed.map, normalMap: dressed.normalMap, color: 0xaaa298, roughness: 0.82 });
const statueMat = new THREE.MeshStandardMaterial({ map: dressed.map, normalMap: dressed.normalMap, color: 0xc4bcb0, roughness: 0.78 });
const darkStoneMat = new THREE.MeshStandardMaterial({ map: dressed.map, normalMap: dressed.normalMap, color: 0x58524e, roughness: 0.6, metalness: 0.1 });
const floorMat = new THREE.MeshStandardMaterial({ map: floorSet.map, normalMap: floorSet.normalMap, roughness: 0.42, metalness: 0.15 });
const woodMat = new THREE.MeshStandardMaterial({ map: woodSet.map, normalMap: woodSet.normalMap, color: 0xb09080, roughness: 0.62 });
const ironMat = new THREE.MeshStandardMaterial({ color: 0x1c1b1e, roughness: 0.5, metalness: 0.85 });
const goldMat = new THREE.MeshStandardMaterial({ color: 0xb08a4a, roughness: 0.32, metalness: 1.0 });
const bronzeMat = new THREE.MeshStandardMaterial({ color: 0x6a4a28, roughness: 0.4, metalness: 0.9 });
const waxMat = new THREE.MeshStandardMaterial({ color: 0xe8dcc0, roughness: 0.55, emissive: 0x2a1808 });
const velvetMat = new THREE.MeshStandardMaterial({ color: 0x3a060c, roughness: 0.95, normalMap: clothSet.normalMap });
const ceilingMat = new THREE.MeshStandardMaterial({ map: ashlar.map, normalMap: ashlar.normalMap, color: 0x5a5458, roughness: 0.95, side: THREE.DoubleSide });
const flameMat = new THREE.MeshBasicMaterial({ color: 0xffb860, fog: true });
const flameCoreMat = new THREE.MeshBasicMaterial({ color: 0xfff1c8 });
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
for (let i = 0; i < 6; i++) { const lz = -6 + i * 3.5 * (i % 2 ? 1 : -1); box(1.2, 0.03, 2.2, darkStoneMat, (i % 2 ? 1 : -1) * 12.8, FY + 0.015, lz, staticG, false); }

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
  const gl = makeGlow(0x9a70ff, r * 3.5, 0.15); gl.position.z = dir * -0.6; g.add(gl);
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
  const gl = makeGlow(0x40507a, 7, 0.08); gl.position.set(0, 4, 0.3); g.add(gl);
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
box(4.72, 0.5, 1.92, velvetMat, 0, 0.92, 0.01, altarG, false);
box(4.0, 0.02, 1.4, cloth(0xb0a896), 0, 1.18, 0, altarG, false);
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
  const g = new THREE.Group(); g.position.set(0, FY + 15, z); staticG.add(g);
  [0, 1, 2].forEach(k => { const a = k / 3 * Math.PI * 2; cyl(0.015, 0.015, 24, 4, ironMat, Math.cos(a) * 1.9, 12, Math.sin(a) * 1.9, g, false); });
  const r1 = new THREE.Mesh(new THREE.TorusGeometry(2.0, 0.06, 6, 40), ironMat); r1.rotation.x = Math.PI / 2; g.add(r1);
  const r2 = new THREE.Mesh(new THREE.TorusGeometry(2.0, 0.04, 6, 40), ironMat); r2.rotation.x = Math.PI / 2; r2.position.y = 0.35; g.add(r2);
  for (let i = 0; i < 16; i++) { const a = i / 16 * Math.PI * 2; cyl(0.015, 0.015, 0.35, 4, ironMat, Math.cos(a) * 2, 0.17, Math.sin(a) * 2, g, false);
    if (i % 2) { const f = new THREE.Mesh(new THREE.ConeGeometry(0.06, 0.3, 4), ironMat); f.position.set(Math.cos(a) * 2, 0.55, Math.sin(a) * 2); g.add(f); }
    else addCandle(g, Math.cos(a) * 2, 0.36, Math.sin(a) * 2, 0.28, 1.2); }
  const l = new THREE.PointLight(0xffa04a, 1.1, 18, 2); l.position.y = -0.4; g.add(l); flickerLights.push({ light: l, base: 1.1, phase: z, optional: true });
}
corona(-12); corona(13);

// =====================================================================
// STATUE E SCULTURE (statiche)
// =====================================================================
function makeRobe(h, r, folds = 9, tatter = 0) {
  const prof = [[0.001, 0], [r, 0], [r * 0.98, 0.05], [r * 0.88, h * 0.3], [r * 0.73, h * 0.55], [r * 0.6, h * 0.74], [r * 0.56, h * 0.82], [r * 0.38, h * 0.9], [0.001, h * 0.93]];
  const g = new THREE.LatheGeometry(prof.map(p => new V2(p[0], p[1])), 36), pos = g.attributes.position;
  for (let i = 0; i < pos.count; i++) { const x = pos.getX(i), y = pos.getY(i), z = pos.getZ(i), a = Math.atan2(z, x), f = 1 - y / h;
    const k = 1 + (Math.sin(a * folds) * 0.6 + Math.sin(a * folds * 2.3 + 1) * 0.4) * 0.09 * f; let yy = y;
    if (tatter && y < h * 0.12) yy = y + Math.abs(Math.sin(a * 11)) * tatter + Math.random() * tatter * 0.3;
    pos.setXYZ(i, x * k, yy, z * k); }
  g.computeVertexNormals(); uvScale(g, 3, 2); return g;
}
function hoodedFigure({ h = 1.9, r = 0.5, mat, faceMat = darkFaceMat, tatter = 0, sleeves = true }) {
  const g = new THREE.Group();
  const robe = new THREE.Mesh(makeRobe(h, r, 9, tatter), mat); robe.castShadow = true; robe.receiveShadow = true; g.add(robe);
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
box(11.2, 0.12, 11.2, goldMat, 0, PL - 0.06, 0, staticG, false);
[-1, 1].forEach(s => [-1, 1].forEach(t => { const c = cyl(0.3, 0.35, 1.4, 8, stoneMat, s * 5.5, FY + 0.7, t * 5.5, staticG); }));

// =====================================================================
// LUCI
// =====================================================================
const ambientLight = new THREE.AmbientLight(0x1c1724, 0.35); scene.add(ambientLight);
const hemi = new THREE.HemisphereLight(0x2a2840, 0x0a0806, 0.25); scene.add(hemi);
const moon = new THREE.DirectionalLight(0x7d8fbf, 0.25); moon.position.set(32, 42, 10); moon.target.position.set(-2, FY, 0);
moon.castShadow = true; moon.shadow.mapSize.set(2048, 2048); Object.assign(moon.shadow.camera, { left: -32, right: 32, top: 32, bottom: -32, near: 1, far: 120 }); moon.shadow.bias = -0.0008;
scene.add(moon, moon.target);
const boardSpot = new THREE.SpotLight(0xffc27a, 3.0, 32, 0.44, 0.65, 1.2); boardSpot.position.set(0, 17, 1.5); boardSpot.target.position.set(0, 0, 0);
boardSpot.castShadow = true; boardSpot.shadow.mapSize.set(2048, 2048); boardSpot.shadow.camera.near = 6; boardSpot.shadow.camera.far = 30; boardSpot.shadow.bias = -0.0005;
scene.add(boardSpot, boardSpot.target);
[[-4.95, -4.95], [4.95, -4.95], [-4.95, 4.95], [4.95, 4.95]].forEach(([x, z], i) => { const l = new THREE.PointLight(0xff9a40, 0.8, 9, 2); l.position.set(x, PL + 3.1, z); scene.add(l); flickerLights.push({ light: l, base: 0.8, phase: i * 2.1 }); });
const altarLight = new THREE.PointLight(0xffa850, 1.5, 14, 2); altarLight.position.set(0, SY + 2.3, -23.4); scene.add(altarLight); flickerLights.push({ light: altarLight, base: 1.5, phase: 3 });
const reredosLight = new THREE.SpotLight(0xffb070, 1.2, 16, 0.6, 0.8, 1.5); reredosLight.position.set(0, SY + 9, -20); reredosLight.target.position.set(0, SY + 3, -27); scene.add(reredosLight, reredosLight.target);

// =====================================================================
// FASCI DI LUCE, PULVISCOLO, INCENSO
// =====================================================================
const shaftG = new THREE.Group(); scene.add(shaftG); const shafts = [];
function addShaft(from, to, r1, r2, color, op) {
  const len = from.distanceTo(to);
  const m = new THREE.Mesh(new THREE.CylinderGeometry(r1, r2, len, 24, 1, true), new THREE.MeshBasicMaterial({ color, transparent: true, opacity: op, blending: THREE.AdditiveBlending, depthWrite: false, side: THREE.DoubleSide, fog: true }));
  m.position.copy(from).add(to).multiplyScalar(0.5); m.quaternion.setFromUnitVectors(new THREE.Vector3(0, -1, 0), to.clone().sub(from).normalize());
  shaftG.add(m); shafts.push({ mesh: m, base: op, phase: Math.random() * 6 });
}
bayZ.forEach(z => addShaft(new THREE.Vector3(NAVE - 0.3, FY + 17, z), new THREE.Vector3(-1.5, FY, z + 1.2), 1.2, 3.2, 0xbfd0ff, 0.022));
addShaft(new THREE.Vector3(0, FY + 28, -LEN + 1), new THREE.Vector3(0, FY, -14), 2.5, 5, 0xc0a0ff, 0.018);
addShaft(new THREE.Vector3(0, 16.5, 1.4), new THREE.Vector3(0, 0, 0), 0.4, 4.4, 0xffd7a0, 0.03);

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
  const pane = new THREE.Mesh(new THREE.BoxGeometry(0.12, 0.2, 0.12), new THREE.MeshBasicMaterial({ color: 0xffa040, transparent: true, opacity: 0.35, fog: true })); g.add(pane);
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
  [6.95, -10.3], [7.0, -3], [6.95, 4], [6.4, 7.7], [0, 8.1], [-6.4, 7.7], [-6.95, 4], [-7.0, -3], [-6.95, -10.3], [0, -10.8]
].map(([x, z]) => new THREE.Vector3(x, FY, z)), true, 'centripetal');
const monks = []; let censerSmokeSrc = new THREE.Vector3(), leadLanternLight = null;
for (let i = 0; i < 6; i++) {
  const f = hoodedFigure({ h: 1.85 + Math.random() * 0.12, r: 0.5, mat: i === 0 ? habitBrown : (i % 2 ? habitBlack : habitBrown) });
  const m = { ...f, u: i * 0.055, speed: 0.0105, censer: null, lantern: null };
  if (i === 0) { const c = censer(); c.pivot.position.set(0, f.h * 0.58, 0.62); f.g.add(c.pivot); m.censer = c; }
  if (i === 1 || i === 5) { const l = lantern(); l.position.set(0.18, f.h * 0.42, 0.62); f.g.add(l); m.lantern = l;
    if (i === 1) { leadLanternLight = new THREE.PointLight(0xff9a40, 0.7, 6, 2); leadLanternLight.position.copy(l.position); f.g.add(leadLanternLight); flickerLights.push({ light: leadLanternLight, base: 0.7, phase: 5, optional: true }); } }
  figuresG.add(f.g); monks.push(m);
}

// Vescovo all'altare
const bishop = (() => {
  const g = new THREE.Group(); g.position.set(0, SY, -25.5); figuresG.add(g);
  const alb = new THREE.Mesh(makeRobe(1.95, 0.48, 12), albMat); alb.castShadow = true; g.add(alb);
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

// Spettro incappucciato nelle navate laterali
const ghostMat = new THREE.MeshStandardMaterial({ color: 0x6a82a8, emissive: 0x18243a, transparent: true, opacity: 0.32, roughness: 1, side: THREE.DoubleSide, depthWrite: false });
const wraith = (() => { const f = hoodedFigure({ h: 2.1, r: 0.55, mat: ghostMat, tatter: 0.35 });
  f.g.traverse(o => { if (o.isMesh) o.castShadow = false; });
  [-1, 1].forEach(s => { const e = makeGlow(0x9fd0ff, 0.12, 1); e.position.set(s * 0.07, 0, 0.22); f.head.add(e); });
  const aura = makeGlow(0x6080c0, 3.5, 0.18); aura.position.y = 1.1; f.g.add(aura);
  creaturesG.add(f.g); return f; })();

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
// SCACCHIERA E PEZZI
// =====================================================================
function createSkullMesh() { const g = new THREE.Group(); const cr = new THREE.Mesh(new THREE.SphereGeometry(0.2, 14, 12)); cr.scale.set(1, 1.1, 1.2); g.add(cr);
  const jaw = new THREE.Mesh(new THREE.BoxGeometry(0.18, 0.12, 0.18)); jaw.position.set(0, -0.12, 0.05); g.add(jaw);
  [-0.07, 0.07].forEach(x => { const e = new THREE.Mesh(new THREE.SphereGeometry(0.05, 8, 8), new THREE.MeshBasicMaterial({ color: 0 })); e.position.set(x, 0.02, 0.16); e.userData.keepMat = true; g.add(e); }); return g; }
function createBase(r, h) { const g = new THREE.Group(); const ring = new THREE.Mesh(new THREE.CylinderGeometry(r, r * 1.15, h, 20)); ring.position.y = h / 2; g.add(ring);
  const rim = new THREE.Mesh(new THREE.TorusGeometry(r * 1.08, 0.03, 6, 24)); rim.rotation.x = Math.PI / 2; rim.position.y = 0.03; g.add(rim);
  for (let i = 0; i < 8; i++) { const a = i / 8 * Math.PI * 2; const v = new THREE.Mesh(new THREE.TorusGeometry(r * 0.7, 0.03, 6, 12, Math.PI)); v.rotation.x = Math.PI / 2 + 0.3; v.rotation.y = a; v.position.set(Math.cos(a) * r * 0.3, h + 0.2, Math.sin(a) * r * 0.3); g.add(v); } return g; }
function spikes(g, n, r, y, h) { for (let i = 0; i < n; i++) { const a = i / n * Math.PI * 2; const s = new THREE.Mesh(new THREE.ConeGeometry(0.035, h, 4)); s.position.set(Math.cos(a) * r, y, Math.sin(a) * r); s.rotation.z = Math.cos(a) * -0.35; s.rotation.x = Math.sin(a) * 0.35; g.add(s); } }
function createPawn() { const g = new THREE.Group(); g.add(createBase(0.32, 0.15)); const s = new THREE.Mesh(new THREE.ConeGeometry(0.2, 0.7, 12)); s.position.y = 0.5; g.add(s); const k = createSkullMesh(); k.position.y = 0.95; k.scale.setScalar(0.8); g.add(k); spikes(g, 4, 0.12, 1.15, 0.2); return g; }
function createRook() { const g = new THREE.Group(); g.add(createBase(0.38, 0.18)); const t = new THREE.Mesh(new THREE.CylinderGeometry(0.28, 0.34, 0.8, 16)); t.position.y = 0.55; g.add(t); for (let i = 0; i < 3; i++) { const r = new THREE.Mesh(new THREE.TorusGeometry(0.3, 0.04, 6, 16)); r.rotation.x = Math.PI / 2 + 0.2; r.position.y = 0.3 + i * 0.25; g.add(r); } const c = new THREE.Mesh(new THREE.CylinderGeometry(0.36, 0.28, 0.25, 8)); c.position.y = 1.05; g.add(c); for (let i = 0; i < 6; i++) { const a = i / 6 * Math.PI * 2; const m = new THREE.Mesh(new THREE.BoxGeometry(0.1, 0.14, 0.1)); m.position.set(Math.cos(a) * 0.3, 1.24, Math.sin(a) * 0.3); g.add(m); } return g; }
function createKnight() { const g = new THREE.Group(); g.add(createBase(0.38, 0.18)); const n = new THREE.Mesh(new THREE.TorusGeometry(0.32, 0.12, 8, 16, Math.PI * 0.85)); n.rotation.z = -Math.PI / 3; n.position.set(-0.08, 0.6, 0); g.add(n); const h = new THREE.Mesh(new THREE.ConeGeometry(0.16, 0.5, 8)); h.rotation.z = Math.PI / 2 + 0.3; h.position.set(0.12, 0.88, 0); g.add(h); for (let i = 0; i < 5; i++) { const ho = new THREE.Mesh(new THREE.ConeGeometry(0.04, 0.25, 4)); ho.position.set(-0.15 + i * 0.05, 0.5 + i * 0.1, 0); ho.rotation.z = -0.5 - i * 0.1; g.add(ho); } g.rotation.y = -Math.PI / 2; const w = new THREE.Group(); w.add(g); return w; }
function createBishop() { const g = new THREE.Group(); g.add(createBase(0.38, 0.18)); const s = new THREE.Mesh(new THREE.CylinderGeometry(0.22, 0.3, 0.6, 12)); s.position.y = 0.48; g.add(s); const k = createSkullMesh(); k.position.y = 0.9; k.scale.setScalar(1.1); g.add(k); const m = new THREE.Mesh(new THREE.ConeGeometry(0.24, 0.6, 8)); m.position.y = 1.35; m.scale.set(1, 1, 0.7); g.add(m); return g; }
function createQueen() { const g = new THREE.Group(); g.add(createBase(0.42, 0.2)); const b = new THREE.Mesh(new THREE.CylinderGeometry(0.18, 0.35, 1.0, 16)); b.position.y = 0.7; g.add(b); for (let i = 0; i < 5; i++) { const s = new THREE.Mesh(new THREE.TorusGeometry(0.24 - i * 0.02, 0.035, 6, 14)); s.rotation.x = Math.PI / 2 + 0.3; s.position.y = 0.35 + i * 0.18; g.add(s); } const k = createSkullMesh(); k.position.y = 1.32; k.scale.setScalar(0.9); g.add(k); spikes(g, 6, 0.18, 1.6, 0.3); return g; }
function createKing() { const g = new THREE.Group(); g.add(createBase(0.45, 0.2)); const b = new THREE.Mesh(new THREE.CylinderGeometry(0.22, 0.38, 1.1, 16)); b.position.y = 0.75; g.add(b); for (let i = 0; i < 6; i++) { const s = new THREE.Mesh(new THREE.TorusGeometry(0.28 - i * 0.02, 0.04, 6, 14)); s.rotation.x = Math.PI / 2 + 0.2; s.position.y = 0.35 + i * 0.18; g.add(s); } const k = createSkullMesh(); k.position.y = 1.45; k.scale.setScalar(1.15); g.add(k); [-1, 1].forEach(sd => { const hb = new THREE.Mesh(new THREE.TorusGeometry(0.2, 0.05, 6, 12, Math.PI * 0.7)); hb.position.set(sd * 0.18, 1.65, 0); hb.rotation.z = sd * -0.8; g.add(hb); const ht = new THREE.Mesh(new THREE.ConeGeometry(0.05, 0.35, 4)); ht.position.set(sd * 0.32, 1.85, 0.05); ht.rotation.z = sd * -0.5; g.add(ht); }); const cv = new THREE.Mesh(new THREE.BoxGeometry(0.04, 0.3, 0.04)); cv.position.y = 1.95; const ch = new THREE.Mesh(new THREE.BoxGeometry(0.18, 0.04, 0.04)); ch.position.y = 2.02; g.add(cv, ch); return g; }
const pieceGeometries = { p: createPawn(), r: createRook(), n: createKnight(), b: createBishop(), q: createQueen(), k: createKing() };
const pieceMaterials = { w: new THREE.MeshStandardMaterial(), b: new THREE.MeshStandardMaterial() };
const PIECE_SETS = {
  ivory: { w: [0xe2d8c4, 0.38, 0.1], b: [0x141216, 0.28, 0.45] },
  metal: { w: [0xc4c8d0, 0.25, 1.0], b: [0x7a4e24, 0.35, 0.95] },
  bone: { w: [0xc8bc9c, 0.75, 0.0], b: [0x070609, 0.1, 0.35] } };
function applyPieceSet() { const s = PIECE_SETS[S.pieceSet] || PIECE_SETS.ivory; ['w', 'b'].forEach(c => { const m = pieceMaterials[c]; m.color.setHex(s[c][0]); m.roughness = s[c][1]; m.metalness = s[c][2]; m.needsUpdate = true; }); }

const boardGroup = new THREE.Group(); scene.add(boardGroup);
const whiteTileMat = new THREE.MeshStandardMaterial({ map: marbleW, roughness: 0.22, metalness: 0.1 });
const blackTileMat = new THREE.MeshStandardMaterial({ map: marbleB, roughness: 0.18, metalness: 0.35 });
function applyBoardStyle() {
  const set = (m, map, c, r, me) => { m.map = map; m.color.setHex(c); m.roughness = r; m.metalness = me; m.needsUpdate = true; };
  if (S.boardStyle === 'obsidian') { set(whiteTileMat, dressed.map, 0x9a948c, 0.7, 0.05); set(blackTileMat, marbleB, 0x403850, 0.06, 0.6); }
  else if (S.boardStyle === 'bone') { set(whiteTileMat, dressed.map, 0xd8c8a0, 0.6, 0); set(blackTileMat, woodSet.map, 0x6a4a38, 0.4, 0.05); }
  else { set(whiteTileMat, marbleW, 0xffffff, 0.22, 0.1); set(blackTileMat, marbleB, 0xffffff, 0.18, 0.35); }
}
const pedestal = new THREE.Mesh(new THREE.BoxGeometry(8.4, 0.5, 8.4), blackTileMat); pedestal.position.y = -0.3; pedestal.receiveShadow = true; pedestal.castShadow = true; boardGroup.add(pedestal);
const frameM = new THREE.Mesh(new THREE.BoxGeometry(8.1, 0.12, 8.1), goldMat); frameM.position.y = -0.02; boardGroup.add(frameM);
const tileSize = 0.95, boardOffset = 8 * tileSize / 2 - tileSize / 2, tiles = {};
const tileGeo = new THREE.BoxGeometry(tileSize * 0.97, 0.15, tileSize * 0.97);
for (let row = 0; row < 8; row++) for (let col = 0; col < 8; col++) {
  const t = new THREE.Mesh(tileGeo, (row + col) % 2 ? blackTileMat : whiteTileMat); t.position.set(col * tileSize - boardOffset, 0, row * tileSize - boardOffset); t.receiveShadow = true;
  const sq = String.fromCharCode(97 + col) + (8 - row); t.userData = { square: sq }; boardGroup.add(t); tiles[sq] = t;
}
const highlightMats = {
  select: new THREE.MeshBasicMaterial({ color: 0xd4a017, transparent: true, opacity: 0.55, depthWrite: false }),
  move: new THREE.MeshBasicMaterial({ color: 0x3a9a5a, transparent: true, opacity: 0.5, depthWrite: false }),
  capture: new THREE.MeshBasicMaterial({ color: 0xb02020, transparent: true, opacity: 0.6, depthWrite: false }),
  last: new THREE.MeshBasicMaterial({ color: 0xa8854f, transparent: true, opacity: 0.28, depthWrite: false }) };
const activeHighlights = [], lastMarks = [], hlGeo = new THREE.PlaneGeometry(tileSize * 0.94, tileSize * 0.94);
function clearHighlights() { activeHighlights.forEach(h => scene.remove(h)); activeHighlights.length = 0; }
function mark(sq, type, list) { const t = tiles[sq]; if (!t) return; const h = new THREE.Mesh(hlGeo, highlightMats[type]); h.rotation.x = -Math.PI / 2; h.position.set(t.position.x, type === 'last' ? 0.079 : 0.083, t.position.z); scene.add(h); list.push(h); }
function addHighlight(sq, type) { if (!S.highlights && type !== 'select') return; mark(sq, type, activeHighlights); }
function updateLastMarks() { lastMarks.forEach(h => scene.remove(h)); lastMarks.length = 0; if (S.lastmove && lastMove) { mark(lastMove.from, 'last', lastMarks); mark(lastMove.to, 'last', lastMarks); } }

const pieceObjects = {};
function spawnPiece(type, color, sq) {
  const mesh = pieceGeometries[type].clone(), cast = renderer.shadowMap.enabled;
  mesh.traverse(c => { if (c.isMesh && !c.userData.keepMat) { c.material = pieceMaterials[color]; c.castShadow = cast; c.receiveShadow = true; } });
  const t = tiles[sq]; mesh.position.set(t.position.x, 0.08, t.position.z); if (color === 'b') mesh.rotation.y = Math.PI;
  scene.add(mesh); pieceObjects[sq] = mesh;
}
function renderBoardState() {
  Object.keys(pieceObjects).forEach(sq => { scene.remove(pieceObjects[sq]); delete pieceObjects[sq]; });
  const b = game.board(); for (let r = 0; r < 8; r++) for (let c = 0; c < 8; c++) if (b[r][c]) spawnPiece(b[r][c].type, b[r][c].color, String.fromCharCode(97 + c) + (8 - r));
  updateLastMarks(); updateUI();
}
const sparks = [];
function burst(x, z, color) { for (let i = 0; i < 16; i++) { const s = makeGlow(color, 0.25, 1); s.position.set(x, 0.2, z); scene.add(s); sparks.push({ s, v: new THREE.Vector3((Math.random() - 0.5) * 0.07, 0.03 + Math.random() * 0.05, (Math.random() - 0.5) * 0.07), life: 1 }); } }
function animatePieceMove(from, to, cb) {
  const m = pieceObjects[from]; if (!m) { cb && cb(); return; }
  const tt = tiles[to], sp = m.position.clone(), ep = new THREE.Vector3(tt.position.x, 0.08, tt.position.z); let p = 0; const dur = Math.round(20 / S.animSpeed);
  (function a() { p++; const t = p / dur, e = t < 0.5 ? 2 * t * t : 1 - Math.pow(-2 * t + 2, 2) / 2; m.position.lerpVectors(sp, ep, e); m.position.y = 0.08 + Math.sin(t * Math.PI) * 0.7;
    if (p < dur) requestAnimationFrame(a); else { m.position.copy(ep); cb && cb(); } })();
}

// =====================================================================
// INTERAZIONE
// =====================================================================
const raycaster = new THREE.Raycaster(), mouse = new THREE.Vector2();
function getSquare(e) { mouse.x = e.clientX / innerWidth * 2 - 1; mouse.y = -(e.clientY / innerHeight) * 2 + 1; raycaster.setFromCamera(mouse, camera);
  for (const i of raycaster.intersectObjects(boardGroup.children)) if (i.object.userData.square) return i.object.userData.square; return null; }
function handleSquareClick(sq) {
  if (game.game_over() || isAiThinking || (gameMode === 'ai' && game.turn() === 'b')) return;
  if (selectedSquare) { if (selectedSquare === sq) { selectedSquare = null; clearHighlights(); return; } const mv = legalMoves.find(m => m.to === sq); if (mv) { executeMove(mv); return; } }
  const p = game.get(sq);
  if (p && p.color === game.turn()) { selectedSquare = sq; clearHighlights(); addHighlight(sq, 'select'); legalMoves = game.moves({ square: sq, verbose: true }); legalMoves.forEach(m => addHighlight(m.to, m.captured ? 'capture' : 'move')); }
  else { selectedSquare = null; clearHighlights(); }
}
let pendingPromotionMove = null;
function executeMove(mv) {
  if (!mv.promotion && (mv.flags.includes('p') || (mv.piece === 'p' && (mv.to[1] === '8' || mv.to[1] === '1')))) {
    if (gameMode === 'ai' && mv.color === 'b') mv.promotion = 'q'; else { pendingPromotionMove = mv; document.getElementById('promotion-modal').classList.remove('hidden'); return; } }
  const cap = !!mv.captured; clearHighlights(); selectedSquare = null;
  animatePieceMove(mv.from, mv.to, () => {
    const res = game.move({ from: mv.from, to: mv.to, promotion: mv.promotion }); if (!res) return;
    lastMove = { from: mv.from, to: mv.to };
    const t = tiles[mv.to]; burst(t.position.x, t.position.z, cap ? 0xc03020 : 0xd8a860);
    cap ? audio.playCapture() : audio.playMove();
    document.getElementById('status-card').classList.toggle('in-check-alert', game.in_check()); if (game.in_check()) audio.playCheck();
    renderBoardState();
    if (isCinemaMode && !game.game_over()) setCameraPreset(game.turn() === 'w' ? 'white' : 'black');
    if (game.game_over()) handleGameOverCondition(); else if (gameMode === 'ai' && game.turn() === 'b') triggerAiTurn();
  });
}
function triggerAiTurn() { isAiThinking = true; document.getElementById('game-status').innerText = 'IA in riflessione…'; setTimeout(() => { const m = getBestMoveAI(); isAiThinking = false; if (m) executeMove(m); }, 600); }
function handleGameOverCondition() {
  clearInterval(timerInterval); let title = 'Partita terminata', desc = 'La partita è giunta a conclusione.';
  if (game.in_checkmate()) { const w = game.turn() === 'w' ? 'Nero' : 'Bianco'; title = 'Scacco matto!'; desc = `Il ${w} trionfa nella cattedrale.`; if (typeof confetti === 'function') confetti({ particleCount: 90, spread: 60, origin: { y: 0.6 }, colors: ['#a8854f', '#5c4426', '#d8c8a8'] }); }
  else if (game.in_draw()) { title = 'Patta'; desc = 'Le forze si equivalgono.'; }
  triggerGameOver(title, desc);
}
function triggerGameOver(t, d) { audio.bell(); document.getElementById('gameover-title').innerText = t; document.getElementById('gameover-desc').innerText = d; document.getElementById('gameover-modal').classList.remove('hidden'); }
let downPos = null;
renderer.domElement.addEventListener('pointerdown', e => { downPos = { x: e.clientX, y: e.clientY }; controls.autoRotate = false; });
renderer.domElement.addEventListener('pointerup', e => { controls.autoRotate = S.autorotate; if (!downPos || Math.hypot(e.clientX - downPos.x, e.clientY - downPos.y) > 8) return; const sq = getSquare(e); if (sq) handleSquareClick(sq); });

const pieceIcons = { p: '♟', r: '♜', n: '♞', b: '♝', q: '♛', k: '♚' };
function updateUI() {
  const s = document.getElementById('game-status'), c = document.getElementById('check-status');
  if (game.in_checkmate()) { s.innerText = `Matto! Vince il ${game.turn() === 'w' ? 'Nero' : 'Bianco'}`; s.className = 'text-red-600'; }
  else if (game.in_draw()) { s.innerText = 'Patta'; s.className = 'text-amber-400'; }
  else { s.innerText = game.turn() === 'w' ? 'Bianco' : 'Nero'; s.className = 'text-amber-500'; }
  if (game.in_check()) { c.innerText = 'In scacco!'; c.className = 'text-red-600 animate-pulse'; } else { c.innerText = 'Sicuro'; c.className = 'text-emerald-500/80'; }
  const cw = [], cb = []; game.history({ verbose: true }).forEach(m => { if (m.captured) (m.color === 'w' ? cb : cw).push(m.captured); });
  document.getElementById('graveyard-white').innerHTML = cw.map(p => pieceIcons[p]).join(' ');
  document.getElementById('graveyard-black').innerHTML = cb.map(p => pieceIcons[p]).join(' ');
  const log = document.getElementById('move-log'), h = game.history(); log.innerHTML = h.length ? '' : '<div class="italic opacity-50">I portali sono aperti. Muove il Bianco.</div>';
  for (let i = 0; i < h.length; i += 2) { const r = document.createElement('div'); r.className = 'flex justify-between border-b border-amber-900/20 py-0.5'; r.innerHTML = `<span class="opacity-50">${i / 2 + 1}.</span><span>${h[i]}</span><span class="opacity-70">${h[i + 1] || ''}</span>`; log.appendChild(r); }
  log.scrollTop = log.scrollHeight;
}

// =====================================================================
// TEMI E APPLICAZIONE IMPOSTAZIONI
// =====================================================================
const THEMES = {
  sanctum: { bg: 0x040306, amb: [0x1c1724, 0.32], hemi: [0x2a2840, 0x0a0806, 0.22], moon: [0x7d8fbf, 0.22], spot: [0xffc27a, 3.0], flame: 0xffa040, glass: 0.62, shaft: 0xbfd0ff },
  moonlight: { bg: 0x02040a, amb: [0x121a2a, 0.38], hemi: [0x26365a, 0x05060a, 0.28], moon: [0xa8c4ff, 0.8], spot: [0xc4d8ff, 2.2], flame: 0xffb070, glass: 0.8, shaft: 0xcfe0ff },
  vespers: { bg: 0x07040a, amb: [0x241a2c, 0.38], hemi: [0x4a2a50, 0x0a0606, 0.28], moon: [0xff9050, 0.55], spot: [0xffb060, 2.8], flame: 0xffa040, glass: 1.05, shaft: 0xffb080 },
  crypt: { bg: 0x060102, amb: [0x220c10, 0.32], hemi: [0x301018, 0x080202, 0.18], moon: [0x602030, 0.18], spot: [0xff5a3a, 2.6], flame: 0xff4a20, glass: 0.4, shaft: 0xff7070 } };
function applyTheme() {
  const t = THEMES[S.theme] || THEMES.sanctum;
  scene.background.setHex(t.bg); scene.fog.color.setHex(t.bg);
  ambientLight.color.setHex(t.amb[0]); ambientLight.intensity = t.amb[1];
  hemi.color.setHex(t.hemi[0]); hemi.groundColor.setHex(t.hemi[1]); hemi.intensity = t.hemi[2];
  moon.color.setHex(t.moon[0]); moon.intensity = t.moon[1]; boardSpot.color.setHex(t.spot[0]); boardSpot.intensity = t.spot[1];
  flameMat.color.setHex(t.flame); flames.forEach(f => f.glow.material.color.setHex(t.flame)); flickerLights.forEach(f => f.light.color.setHex(t.flame));
  glassMats.forEach(m => m.color.setScalar(t.glass)); shafts.forEach(s => s.mesh.material.color.setHex(t.shaft));
  scheduleEnv();
}
let envTex = null, envTimer = null;
function buildEnv() {
  const hidden = [boardGroup, figuresG, creaturesG, dust, smokeG, shaftG]; const vis = hidden.map(o => o.visible); hidden.forEach(o => o.visible = false);
  const pieces = Object.values(pieceObjects); pieces.forEach(p => p.visible = false);
  scene.environment = null; const pm = new THREE.PMREMGenerator(renderer); if (envTex) envTex.dispose();
  envTex = pm.fromScene(scene, 0.035, 0.1, 100).texture; pm.dispose();
  hidden.forEach((o, i) => o.visible = vis[i]); pieces.forEach(p => p.visible = true);
  scene.environment = S.reflections ? envTex : null;
}
function scheduleEnv() { clearTimeout(envTimer); envTimer = setTimeout(buildEnv, 250); }
function applySettings() {
  const pr = { low: 0.75, med: 1, high: Math.min(devicePixelRatio, 2) }[S.quality] || 1; renderer.setPixelRatio(pr); renderer.setSize(innerWidth, innerHeight);
  const shadowsOn = S.shadows !== 'off';
  if (renderer.shadowMap.enabled !== shadowsOn || (shadowsOn && renderer.shadowMap.type !== (S.shadows === 'soft' ? THREE.PCFSoftShadowMap : THREE.PCFShadowMap))) {
    renderer.shadowMap.enabled = shadowsOn; renderer.shadowMap.type = S.shadows === 'soft' ? THREE.PCFSoftShadowMap : THREE.PCFShadowMap;
    scene.traverse(o => { if (o.material) [].concat(o.material).forEach(m => m.needsUpdate = true); }); renderBoardState(); }
  boardSpot.castShadow = shadowsOn; moon.castShadow = shadowsOn && S.quality !== 'low';
  flickerLights.forEach(f => { if (f.optional) f.light.visible = S.quality !== 'low'; });
  scene.environment = S.reflections && envTex ? envTex : null;
  renderer.toneMappingExposure = S.exposure; camera.fov = S.fov; camera.updateProjectionMatrix();
  controls.autoRotate = S.autorotate; scene.fog.density = S.fog;
  shaftG.visible = S.shafts; dust.visible = smokeG.visible = S.dust; figuresG.visible = S.figures; creaturesG.visible = S.creatures;
  audio.setVolumes(); applyPieceSet(); applyBoardStyle(); updateLastMarks();
}
let currentTheme = S.theme;
function syncSettingsUI() { document.querySelectorAll('[data-key]').forEach(el => { const v = S[el.dataset.key]; if (el.type === 'checkbox') el.checked = !!v; else el.value = v; }); }
document.querySelectorAll('[data-key]').forEach(el => el.addEventListener(el.tagName === 'SELECT' || el.type === 'checkbox' ? 'change' : 'input', () => {
  const k = el.dataset.key; S[k] = el.type === 'checkbox' ? el.checked : el.type === 'range' ? parseFloat(el.value) : el.value;
  if (k === 'theme') applyTheme(); applySettings(); if (k === 'reflections' && S.reflections && !envTex) buildEnv(); saveSettings(); }));
document.querySelectorAll('.tab-btn').forEach(b => b.onclick = () => { document.querySelectorAll('.tab-btn').forEach(x => x.classList.toggle('active', x === b)); document.querySelectorAll('[data-panel]').forEach(p => p.classList.toggle('hidden', p.dataset.panel !== b.dataset.tab)); });
document.getElementById('btn-defaults').onclick = () => { S = { ...DEFAULTS }; saveSettings(); syncSettingsUI(); applyTheme(); applySettings(); };

// =====================================================================
// CONTROLLI UI
// =====================================================================
const $ = id => document.getElementById(id);
$('btn-mode-pvp').onclick = () => { gameMode = 'pvp'; $('btn-mode-pvp').classList.add('active'); $('btn-mode-ai').classList.remove('active'); $('ai-difficulty-container').classList.add('hidden'); $('ai-difficulty-container').classList.remove('flex'); };
$('btn-mode-ai').onclick = () => { gameMode = 'ai'; $('btn-mode-ai').classList.add('active'); $('btn-mode-pvp').classList.remove('active'); $('ai-difficulty-container').classList.remove('hidden'); $('ai-difficulty-container').classList.add('flex'); if (game.turn() === 'b' && !game.game_over()) triggerAiTurn(); };
['easy', 'med', 'hard'].forEach(d => $(`ai-${d}`).onclick = e => { aiDifficulty = d; ['easy', 'med', 'hard'].forEach(x => $(`ai-${x}`).classList.remove('active')); e.currentTarget.classList.add('active'); });
['off', '3m', '5m'].forEach(k => $(`timer-${k}`).onclick = e => { timerMode = k; ['off', '3m', '5m'].forEach(x => $(`timer-${x}`).classList.remove('active')); e.currentTarget.classList.add('active'); const d = $('timer-display-container');
  if (k === 'off') { d.classList.add('hidden'); d.classList.remove('flex'); clearInterval(timerInterval); } else { d.classList.remove('hidden'); d.classList.add('flex'); timeWhite = timeBlack = k === '3m' ? 180 : 300; updateTimerUI(); startTimerInterval(); } });
$('btn-hint').onclick = () => { if (game.game_over() || isAiThinking) return; const m = getBestMoveAI(); if (m) { clearHighlights(); mark(m.from, 'select', activeHighlights); mark(m.to, 'move', activeHighlights); setTimeout(clearHighlights, 3000); } };
$('btn-undo').onclick = () => { if (isAiThinking) return; game.undo(); if (gameMode === 'ai' && game.turn() === 'b') game.undo(); const h = game.history({ verbose: true }); lastMove = h.length ? h[h.length - 1] : null;
  selectedSquare = null; clearHighlights(); $('status-card').classList.remove('in-check-alert'); renderBoardState(); };
$('btn-reset').onclick = () => { game.reset(); lastMove = null; selectedSquare = null; clearHighlights(); $('status-card').classList.remove('in-check-alert'); $('gameover-modal').classList.add('hidden');
  if (timerMode !== 'off') { timeWhite = timeBlack = timerMode === '3m' ? 180 : 300; updateTimerUI(); startTimerInterval(); } renderBoardState(); };
$('btn-play-again').onclick = () => $('btn-reset').click();
$('btn-sound').onclick = () => { audio.soundEnabled = !audio.soundEnabled; $('sound-icon').innerText = audio.soundEnabled ? '🔊' : '🔇'; };
$('btn-ambient').onclick = () => { const a = audio.toggleAmbient(); $('ambient-icon').innerText = a ? '🎶' : '🎵'; $('btn-ambient').classList.toggle('active', a); };
['white', 'black', 'top', 'nave', 'apse', 'cinematic'].forEach(k => $(`cam-${k}`).onclick = e => { ['white', 'black', 'top', 'nave', 'apse', 'cinematic'].forEach(x => $(`cam-${x}`).classList.remove('active')); e.currentTarget.classList.add('active');
  if (k === 'cinematic') { isCinemaMode = true; setCameraPreset(game.turn() === 'w' ? 'white' : 'black'); } else { isCinemaMode = false; setCameraPreset(k); } });
document.querySelectorAll('.promo-btn').forEach(b => b.onclick = e => { if (!pendingPromotionMove) return; const m = pendingPromotionMove; m.promotion = e.currentTarget.dataset.piece; pendingPromotionMove = null; $('promotion-modal').classList.add('hidden'); executeMove(m); });
$('btn-settings-modal').onclick = () => { syncSettingsUI(); $('settings-modal').classList.remove('hidden'); };
$('close-settings').onclick = () => $('settings-modal').classList.add('hidden');
$('settings-modal').addEventListener('click', e => { if (e.target.id === 'settings-modal') $('settings-modal').classList.add('hidden'); });
$('btn-mobile-menu').onclick = () => { const p = $('control-panel'); p.classList.toggle('hidden'); p.classList.toggle('flex'); };
addEventListener('resize', () => { camera.aspect = innerWidth / innerHeight; camera.updateProjectionMatrix(); renderer.setSize(innerWidth, innerHeight); });

// =====================================================================
// AVVIO
// =====================================================================
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
  const dt = Math.min(clock.getDelta(), 0.05), t = clock.elapsedTime, k = reduceMotion ? 0.3 : 1, cm = S.candles;
  controls.update();

  flames.forEach(f => { const fl = 0.82 + Math.sin(t * 11 + f.phase) * 0.08 + Math.sin(t * 27 + f.phase * 1.7) * 0.06 + Math.random() * 0.05;
    f.fg.scale.set(1, fl, 1); f.fg.rotation.z = Math.sin(t * 3 + f.phase) * 0.08; f.glow.scale.setScalar(f.base * fl * Math.max(cm, 0.05)); f.glow.material.opacity = 0.75 * Math.min(cm, 1.2); f.fg.visible = cm > 0.02; });
  flickerLights.forEach(f => { f.light.intensity = f.base * cm * (0.85 + Math.sin(t * 9 + f.phase) * 0.06 + Math.sin(t * 19 + f.phase) * 0.05 + Math.random() * 0.06); });
  shafts.forEach(s => { s.mesh.material.opacity = s.base * (0.75 + Math.sin(t * 0.35 + s.phase) * 0.25); });
  gargoyleEyes.forEach((e, i) => { e.material.opacity = 0.35 + Math.max(0, Math.sin(t * 0.6 + i * 1.3)) * 0.5; });

  if (figuresG.visible) {
    monks.forEach((m, i) => {
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
    const wt = t * 0.045, side = Math.sin(wt * 0.5) > 0 ? 1 : -1;
    const wz = Math.sin(wt * 2) * 20; wraith.g.position.set(side * 13.2, FY + 0.35 + Math.sin(t * 0.9) * 0.15, wz);
    wraith.g.rotation.y = Math.cos(wt * 2) > 0 ? 0 : Math.PI;
    ghostMat.opacity = 0.12 + Math.max(0, Math.sin(t * 0.25)) * 0.26;
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
  for (let i = sparks.length - 1; i >= 0; i--) { const p = sparks[i]; p.s.position.add(p.v); p.v.y -= 0.0025; p.life -= 0.03; p.s.material.opacity = p.life; if (p.life <= 0) { scene.remove(p.s); p.s.material.dispose(); sparks.splice(i, 1); } }
  renderer.render(scene, camera);
}
animateLoop();
</script>
</body>
</html>
