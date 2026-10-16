# Il mio codice di prova

Ecco un esempio di codice HTML che ho scritto:

```<!DOCTYPE html>
<html lang="it" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Sanctum Gothic Cathedral 3D Chess - Dark Fantasy Edition</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts for Gothic Aesthetic -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;600;700;900&family=MedievalSharp&display=swap" rel="stylesheet">
    
    <!-- Three.js and OrbitControls -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
    <!-- Chess.js logic library -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/chess.js/0.10.3/chess.min.js"></script>
    <!-- Canvas Confetti for Victory Screen -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

    <style>
        body {
            font-family: 'Cinzel', serif;
            user-select: none;
            overflow: hidden;
            background-color: #030305;
            color: #e2d7c5;
            touch-action: none;
        }
        .gothic-border {
            border: 1px solid #7c623d;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.9), inset 0 0 10px rgba(124, 98, 61, 0.2);
        }
        .gothic-card {
            background: rgba(10, 8, 14, 0.94);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(163, 131, 78, 0.4);
        }
        .gothic-btn {
            background: linear-gradient(135deg, #1b140e 0%, #352718 100%);
            border: 1px solid #9a7b4f;
            color: #e2d7c5;
            transition: all 0.2s ease;
        }
        .gothic-btn:hover {
            background: linear-gradient(135deg, #352718 0%, #584126 100%);
            border-color: #dfb26d;
            box-shadow: 0 0 12px rgba(223, 178, 109, 0.4);
            color: #ffffff;
        }
        .gothic-btn.active {
            background: linear-gradient(135deg, #664721 0%, #9a7b4f 100%);
            color: #fff;
            box-shadow: 0 0 15px rgba(223, 178, 109, 0.6);
            border-color: #f3d08a;
        }
        @keyframes pulse-gothic {
            0%, 100% { box-shadow: 0 0 15px rgba(239, 68, 68, 0.4), inset 0 0 10px rgba(239, 68, 68, 0.2); }
            50% { box-shadow: 0 0 30px rgba(239, 68, 68, 0.8), inset 0 0 20px rgba(239, 68, 68, 0.5); }
        }
        .in-check-alert {
            animation: pulse-gothic 1.5s infinite;
            border-color: #ef4444 !important;
        }
        ::-webkit-scrollbar {
            width: 4px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(10, 8, 12, 0.6);
        }
        ::-webkit-scrollbar-thumb {
            background: #5a452a;
            border-radius: 2px;
        }
    </style>
</head>
<body class="w-full h-full m-0 p-0 overflow-hidden select-none relative">

    <!-- 3D Canvas Container -->
    <div id="canvas-container" class="absolute inset-0 w-full h-full z-0 cursor-grab active:cursor-grabbing"></div>

    <!-- UI Overlay Container -->
    <div class="relative z-10 w-full h-full pointer-events-none flex flex-col justify-between p-2 sm:p-4 overflow-hidden">
        
        <!-- Top Bar: Title & Mode Settings -->
        <div class="flex flex-col sm:flex-row justify-between items-stretch sm:items-start gap-2.5 w-full">
            
            <!-- Title & Status Card -->
            <div id="status-card" class="gothic-card gothic-border p-2.5 sm:p-3 rounded-lg pointer-events-auto sm:w-72 transition-all duration-300">
                <div class="flex justify-between items-center">
                    <h1 class="text-base sm:text-lg font-black text-amber-500 tracking-wider flex items-center gap-2">
                        <i class="fa-solid fa-chess-king text-amber-600"></i> SANCTUM CHESS
                    </h1>
                    <button id="btn-mobile-menu" class="sm:hidden gothic-btn px-2 py-1 rounded text-xs">
                        <i class="fa-solid fa-gear"></i>
                    </button>
                </div>
                <p class="text-[10px] sm:text-[11px] text-amber-200/70 italic">Dark Fantasy Gothic Edition</p>
                
                <div class="mt-2 pt-2 border-t border-amber-900/50 flex justify-between items-center text-xs">
                    <span class="text-amber-100/70">Turno: <strong id="game-status" class="text-amber-400">Bianco</strong></span>
                    <span class="text-amber-300/80">Stato: <strong id="check-status" class="text-emerald-400 font-semibold">Sicuro</strong></span>
                </div>
            </div>

            <!-- Main Control Panel -->
            <div id="control-panel" class="gothic-card gothic-border p-2.5 sm:p-3 rounded-lg pointer-events-auto flex flex-wrap items-center gap-2.5 transition-all">
                
                <!-- Game Mode -->
                <div class="flex flex-col gap-1">
                    <span class="text-[9px] uppercase font-bold text-amber-500/90">Modalità</span>
                    <div class="flex gap-1 text-xs">
                        <button id="btn-mode-pvp" class="gothic-btn px-2.5 py-1 rounded active">PvP</button>
                        <button id="btn-mode-ai" class="gothic-btn px-2.5 py-1 rounded">vs IA</button>
                    </div>
                </div>

                <!-- AI Difficulty -->
                <div id="ai-difficulty-container" class="hidden flex-col gap-1">
                    <span class="text-[9px] uppercase font-bold text-amber-500/90">Difficoltà</span>
                    <div class="flex gap-1 text-xs">
                        <button id="ai-easy" class="gothic-btn px-2 py-1 rounded">Acolito</button>
                        <button id="ai-med" class="gothic-btn px-2 py-1 rounded active">Vescovo</button>
                        <button id="ai-hard" class="gothic-btn px-2 py-1 rounded">Gran Maestro</button>
                    </div>
                </div>

                <!-- Timers -->
                <div class="flex flex-col gap-1">
                    <span class="text-[9px] uppercase font-bold text-amber-500/90">Tempo (Blitz)</span>
                    <div class="flex gap-1 text-xs">
                        <button id="timer-off" class="gothic-btn px-2 py-1 rounded active" title="Nessun limite">Off</button>
                        <button id="timer-3m" class="gothic-btn px-2 py-1 rounded" title="3 Minuti">3m</button>
                        <button id="timer-5m" class="gothic-btn px-2 py-1 rounded" title="5 Minuti">5m</button>
                    </div>
                </div>

                <!-- Timers Display -->
                <div id="timer-display-container" class="hidden items-center gap-2 text-xs font-mono bg-black/40 px-2.5 py-1 rounded border border-amber-900/40">
                    <span class="text-amber-200">W: <strong id="timer-white" class="text-amber-400">03:00</strong></span>
                    <span class="text-amber-900">|</span>
                    <span class="text-amber-200">B: <strong id="timer-black" class="text-amber-400">03:00</strong></span>
                </div>

                <!-- Action Buttons -->
                <div class="flex items-center gap-1.5 ml-auto">
                    <button id="btn-undo" class="gothic-btn px-2 py-1.5 rounded text-xs flex items-center gap-1" title="Annulla Mossa">
                        <i class="fa-solid fa-rotate-left"></i> <span class="hidden md:inline">Indietro</span>
                    </button>
                    <button id="btn-hint" class="gothic-btn px-2 py-1.5 rounded text-xs flex items-center gap-1 text-amber-300" title="Suggerimento IA">
                        <i class="fa-solid fa-lightbulb"></i> <span class="hidden md:inline">Aiuto</span>
                    </button>
                    <button id="btn-settings-modal" class="gothic-btn px-2 py-1.5 rounded text-xs flex items-center gap-1" title="Impostazioni Grafiche">
                        <i class="fa-solid fa-sliders"></i>
                    </button>
                    <button id="btn-reset" class="gothic-btn px-2 py-1.5 rounded text-xs flex items-center gap-1" title="Nuova Partita">
                        <i class="fa-solid fa-arrows-rotate"></i>
                    </button>
                </div>
            </div>
        </div>

        <!-- Right Side Panel: Graveyard & Move Log -->
        <div class="absolute right-4 top-28 bottom-24 w-60 pointer-events-none hidden xl:flex flex-col gap-3 justify-start z-20">
            <!-- Graveyard -->
            <div class="gothic-card gothic-border p-3 rounded-lg text-xs shadow-xl pointer-events-auto">
                <div class="text-amber-400 font-bold mb-1 border-b border-amber-900/40 pb-1 flex justify-between">
                    <span>Offerte Cadute</span>
                    <i class="fa-solid fa-skull text-amber-700"></i>
                </div>
                <div class="flex flex-col gap-1.5 mt-2">
                    <div class="flex items-center gap-1 min-h-[22px] text-lg text-amber-100 flex-wrap" id="graveyard-white"></div>
                    <div class="flex items-center gap-1 min-h-[22px] text-lg text-stone-300 flex-wrap" id="graveyard-black"></div>
                </div>
            </div>

            <!-- Move Notation Log -->
            <div class="gothic-card gothic-border p-3 rounded-lg text-xs flex flex-col flex-1 shadow-xl overflow-hidden pointer-events-auto">
                <div class="text-amber-400 font-bold mb-1 border-b border-amber-900/40 pb-1 flex justify-between shrink-0">
                    <span>Annali della Cattedrale</span>
                    <i class="fa-solid fa-scroll text-amber-700"></i>
                </div>
                <div id="move-log" class="overflow-y-auto flex-1 font-mono text-[11px] text-amber-200/80 space-y-1 my-1 pr-1">
                    <div class="italic text-amber-500/50">Portali aperti. Muove il Bianco.</div>
                </div>
            </div>
        </div>

        <!-- Bottom Controls Bar: Audio & Cameras -->
        <div class="flex flex-wrap justify-between items-center gap-2 w-full">
            <!-- Sound & Ambient Audio -->
            <div class="gothic-card gothic-border p-1.5 sm:p-2 rounded-lg pointer-events-auto flex items-center gap-2">
                <button id="btn-sound" class="gothic-btn p-2 rounded-md text-sm" title="Effetti Sonori">
                    <i id="sound-icon" class="fa-solid fa-volume-high"></i>
                </button>
                <button id="btn-ambient" class="gothic-btn p-2 rounded-md text-sm" title="Organo Gotico Ambientale">
                    <i id="ambient-icon" class="fa-solid fa-music"></i>
                </button>
            </div>

            <!-- Camera Quick View Presets -->
            <div class="gothic-card gothic-border p-1.5 sm:p-2 rounded-lg pointer-events-auto flex items-center gap-1 text-xs">
                <span class="text-amber-400 font-semibold px-1 hidden sm:inline"><i class="fa-solid fa-video mr-1"></i> Vista:</span>
                <button id="cam-white" class="gothic-btn px-2 sm:px-2.5 py-1.5 rounded active">Bianco</button>
                <button id="cam-black" class="gothic-btn px-2 sm:px-2.5 py-1.5 rounded">Nero</button>
                <button id="cam-top" class="gothic-btn px-2 sm:px-2.5 py-1.5 rounded">Alto</button>
                <button id="cam-cinematic" class="gothic-btn px-2 sm:px-2.5 py-1.5 rounded">Cinema</button>
            </div>
        </div>
    </div>

    <!-- Pawn Promotion Modal -->
    <div id="promotion-modal" class="fixed inset-0 z-50 bg-black/85 backdrop-blur-md hidden flex items-center justify-center p-4">
        <div class="gothic-card gothic-border p-6 rounded-xl text-center max-w-md w-full pointer-events-auto">
            <h2 class="text-xl font-bold text-amber-400 mb-2">Ascesa del Pedone</h2>
            <p class="text-xs text-amber-200/70 mb-6">Scegli il rango gotico superiore per il tuo pedone consacrato</p>
            <div class="grid grid-cols-4 gap-3">
                <button data-piece="q" class="promo-btn gothic-btn p-4 rounded-lg flex flex-col items-center gap-2">
                    <i class="fa-solid fa-chess-queen text-2xl text-amber-400"></i>
                    <span class="text-xs">Regina</span>
                </button>
                <button data-piece="r" class="promo-btn gothic-btn p-4 rounded-lg flex flex-col items-center gap-2">
                    <i class="fa-solid fa-chess-rook text-2xl text-amber-400"></i>
                    <span class="text-xs">Torre</span>
                </button>
                <button data-piece="b" class="promo-btn gothic-btn p-4 rounded-lg flex flex-col items-center gap-2">
                    <i class="fa-solid fa-chess-bishop text-2xl text-amber-400"></i>
                    <span class="text-xs">Alfiere</span>
                </button>
                <button data-piece="n" class="promo-btn gothic-btn p-4 rounded-lg flex flex-col items-center gap-2">
                    <i class="fa-solid fa-chess-knight text-2xl text-amber-400"></i>
                    <span class="text-xs">Cavaliere</span>
                </button>
            </div>
        </div>
    </div>

    <!-- Graphics & Options Settings Modal -->
    <div id="settings-modal" class="fixed inset-0 z-50 bg-black/85 backdrop-blur-md hidden flex items-center justify-center p-4">
        <div class="gothic-card gothic-border p-6 rounded-xl max-w-md w-full pointer-events-auto">
            <div class="flex justify-between items-center mb-4 border-b border-amber-900/50 pb-2">
                <h2 class="text-lg font-bold text-amber-400"><i class="fa-solid fa-sliders mr-2"></i> Impostazioni Cattedrale</h2>
                <button id="close-settings" class="text-amber-400 hover:text-white text-lg"><i class="fa-solid fa-xmark"></i></button>
            </div>
            
            <div class="space-y-4 text-xs">
                <!-- Theme / Lighting Mood -->
                <div class="flex flex-col gap-1.5">
                    <label class="font-bold text-amber-300">Atmosfera di Luce</label>
                    <div class="grid grid-cols-3 gap-2">
                        <button data-theme="sanctum" class="theme-btn gothic-btn p-2 rounded active text-center">Sanctum Dorato</button>
                        <button data-theme="crypt" class="theme-btn gothic-btn p-2 rounded text-center">Cripta Sanguigna</button>
                        <button data-theme="moonlight" class="theme-btn gothic-btn p-2 rounded text-center">Notturno Lunare</button>
                    </div>
                </div>

                <!-- Shadows Quality -->
                <div class="flex flex-col gap-1.5">
                    <label class="font-bold text-amber-300">Ombre e Dettagli 3D</label>
                    <div class="grid grid-cols-2 gap-2">
                        <button data-shadow="high" id="shadow-high" class="gothic-btn p-2 rounded active text-center">Ombre Alte (HQ)</button>
                        <button data-shadow="off" id="shadow-off" class="gothic-btn p-2 rounded text-center">Disattiva (Performance)</button>
                    </div>
                </div>

                <!-- Rules Assistance -->
                <div class="flex items-center justify-between pt-2 border-t border-amber-900/40">
                    <span class="font-bold text-amber-300">Evidenzia Mosse Legali</span>
                    <input type="checkbox" id="toggle-highlights" checked class="accent-amber-600 w-4 h-4 cursor-pointer">
                </div>
            </div>
        </div>
    </div>

    <!-- Game Over Modal -->
    <div id="gameover-modal" class="fixed inset-0 z-50 bg-black/90 backdrop-blur-lg hidden flex items-center justify-center p-4">
        <div class="gothic-card gothic-border p-8 rounded-xl text-center max-w-sm w-full pointer-events-auto shadow-2xl">
            <div class="text-amber-500 text-4xl mb-3"><i class="fa-solid fa-trophy"></i></div>
            <h2 id="gameover-title" class="text-2xl font-bold text-amber-400 mb-1">Scacco Matto!</h2>
            <p id="gameover-desc" class="text-xs text-amber-200/80 mb-6">Il santuario ha decretato il vincitore.</p>
            <button id="btn-play-again" class="gothic-btn w-full py-3 rounded-lg font-bold text-sm tracking-wider">
                RIGIOCA NELLA CATTEDRALE
            </button>
        </div>
    </div>

    <script>
        const container = document.getElementById('canvas-container');
        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0x030305);
        scene.fog = new THREE.FogExp2(0x030305, 0.025);

        const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: false, powerPreference: "high-performance" });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        renderer.shadowMap.enabled = true;
        renderer.shadowMap.type = THREE.PCFSoftShadowMap;
        container.appendChild(renderer.domElement);

        const controls = new THREE.OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;
        controls.dampingFactor = 0.05;
        controls.maxPolarAngle = Math.PI / 2 - 0.04;
        controls.minDistance = 4;
        controls.maxDistance = 32;

        let isCinemaMode = false;

        const cameraPresets = {
            white: { pos: new THREE.Vector3(0, 8.5, 9.5), target: new THREE.Vector3(0, 0, 0) },
            black: { pos: new THREE.Vector3(0, 8.5, -9.5), target: new THREE.Vector3(0, 0, 0) },
            top: { pos: new THREE.Vector3(0, 16, 0.01), target: new THREE.Vector3(0, 0, 0) },
            cinematic: { pos: new THREE.Vector3(11, 6.5, 11), target: new THREE.Vector3(0, 0.8, 0) }
        };

        function setCameraPreset(presetKey, instant = false) {
            const p = cameraPresets[presetKey];
            if (!p) return;
            if (instant) {
                camera.position.copy(p.pos);
                controls.target.copy(p.target);
                controls.update();
                return;
            }
            const startPos = camera.position.clone();
            const startTarget = controls.target.clone();
            let progress = 0;

            function animateCam() {
                progress += 0.04;
                const t = progress < 0.5 ? 4 * progress * progress * progress : 1 - Math.pow(-2 * progress + 2, 3) / 2;
                if (progress <= 1) {
                    camera.position.lerpVectors(startPos, p.pos, t);
                    controls.target.lerpVectors(startTarget, p.target, t);
                    controls.update();
                    requestAnimationFrame(animateCam);
                } else {
                    camera.position.copy(p.pos);
                    controls.target.copy(p.target);
                }
            }
            animateCam();
        }
        camera.position.copy(cameraPresets.white.pos);
        controls.target.copy(cameraPresets.white.target);

        class GothicAudio {
            constructor() {
                this.ctx = null;
                this.soundEnabled = true;
                this.ambientActive = false;
                this.ambientInterval = null;
            }

            init() {
                if (!this.ctx) {
                    this.ctx = new (window.AudioContext || window.webkitAudioContext)();
                }
            }

            playMove() {
                if (!this.soundEnabled) return;
                this.init();
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'triangle';
                osc.frequency.setValueAtTime(160, this.ctx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(70, this.ctx.currentTime + 0.18);
                gain.gain.setValueAtTime(0.25, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.18);
                osc.connect(gain);
                gain.connect(this.ctx.destination);
                osc.start();
                osc.stop(this.ctx.currentTime + 0.18);
            }

            playCapture() {
                if (!this.soundEnabled) return;
                this.init();
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(110, this.ctx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(35, this.ctx.currentTime + 0.28);
                gain.gain.setValueAtTime(0.35, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.28);
                osc.connect(gain);
                gain.connect(this.ctx.destination);
                osc.start();
                osc.stop(this.ctx.currentTime + 0.28);
            }

            playCheck() {
                if (!this.soundEnabled) return;
                this.init();
                [523.25, 659.25, 783.99, 1046.50].forEach((freq, idx) => {
                    const osc = this.ctx.createOscillator();
                    const gain = this.ctx.createGain();
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(freq, this.ctx.currentTime + idx * 0.07);
                    gain.gain.setValueAtTime(0.18, this.ctx.currentTime + idx * 0.07);
                    gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + idx * 0.07 + 0.35);
                    osc.connect(gain);
                    gain.connect(this.ctx.destination);
                    osc.start(this.ctx.currentTime + idx * 0.07);
                    osc.stop(this.ctx.currentTime + idx * 0.07 + 0.35);
                });
            }

            toggleAmbient() {
                this.init();
                this.ambientActive = !this.ambientActive;
                if (this.ambientActive) {
                    const playOrganNote = () => {
                        if (!this.ambientActive) return;
                        const notes = [110, 130.81, 164.81, 196.00, 220.00, 246.94];
                        const note = notes[Math.floor(Math.random() * notes.length)];
                        const osc = this.ctx.createOscillator();
                        const gain = this.ctx.createGain();
                        osc.type = 'sawtooth';
                        osc.frequency.setValueAtTime(note, this.ctx.currentTime);
                        gain.gain.setValueAtTime(0.001, this.ctx.currentTime);
                        gain.gain.linearRampToValueAtTime(0.04, this.ctx.currentTime + 1.2);
                        gain.gain.linearRampToValueAtTime(0.001, this.ctx.currentTime + 4.0);
                        osc.connect(gain);
                        gain.connect(this.ctx.destination);
                        osc.start();
                        osc.stop(this.ctx.currentTime + 4.0);
                    };
                    playOrganNote();
                    this.ambientInterval = setInterval(playOrganNote, 4500);
                } else {
                    if (this.ambientInterval) clearInterval(this.ambientInterval);
                }
                return this.ambientActive;
            }
        }
        const audio = new GothicAudio();

        const game = new Chess();
        let gameMode = 'pvp';
        let aiDifficulty = 'med';
        let selectedSquare = null;
        let legalMoves = [];
        let isAiThinking = false;

        let timerMode = 'off';
        let timeWhite = 180;
        let timeBlack = 180;
        let timerInterval = null;

        function startTimerInterval() {
            if (timerInterval) clearInterval(timerInterval);
            if (timerMode === 'off') return;
            
            timerInterval = setInterval(() => {
                if (game.game_over()) {
                    clearInterval(timerInterval);
                    return;
                }
                if (game.turn() === 'w') {
                    timeWhite--;
                    if (timeWhite <= 0) {
                        timeWhite = 0;
                        clearInterval(timerInterval);
                        triggerGameOver("Tempo Scaduto!", "Il tempo del Bianco si è esaurito. Vittoria del Nero!");
                    }
                } else {
                    timeBlack--;
                    if (timeBlack <= 0) {
                        timeBlack = 0;
                        clearInterval(timerInterval);
                        triggerGameOver("Tempo Scaduto!", "Il tempo del Nero si è esaurito. Vittoria del Bianco!");
                    }
                }
                updateTimerUI();
            }, 1000);
        }

        function updateTimerUI() {
            const formatTime = (secs) => {
                const m = Math.floor(secs / 60).toString().padStart(2, '0');
                const s = (secs % 60).toString().padStart(2, '0');
                return `${m}:${s}`;
            };
            document.getElementById('timer-white').innerText = formatTime(timeWhite);
            document.getElementById('timer-black').innerText = formatTime(timeBlack);
        }

        const pieceValues = { p: 10, n: 30, b: 30, r: 50, q: 90, k: 900 };

        function evaluateBoard(board) {
            let totalEvaluation = 0;
            for (let r = 0; r < 8; r++) {
                for (let c = 0; c < 8; c++) {
                    const piece = board[r][c];
                    if (piece) {
                        const val = pieceValues[piece.type];
                        totalEvaluation += (piece.color === 'w') ? val : -val;
                    }
                }
            }
            return totalEvaluation;
        }

        function minimax(depth, gameInstance, isMaximizing, alpha, beta) {
            if (depth === 0 || gameInstance.game_over()) {
                return evaluateBoard(gameInstance.board());
            }

            const possibleMoves = gameInstance.moves();
            if (isMaximizing) {
                let maxEval = -99999;
                for (let i = 0; i < possibleMoves.length; i++) {
                    gameInstance.move(possibleMoves[i]);
                    const evaluation = minimax(depth - 1, gameInstance, false, alpha, beta);
                    gameInstance.undo();
                    maxEval = Math.max(maxEval, evaluation);
                    alpha = Math.max(alpha, evaluation);
                    if (beta <= alpha) break;
                }
                return maxEval;
            } else {
                let minEval = 99999;
                for (let i = 0; i < possibleMoves.length; i++) {
                    gameInstance.move(possibleMoves[i]);
                    const evaluation = minimax(depth - 1, gameInstance, true, alpha, beta);
                    gameInstance.undo();
                    minEval = Math.min(minEval, evaluation);
                    beta = Math.min(beta, evaluation);
                    if (beta <= alpha) break;
                }
                return minEval;
            }
        }

        function getBestMoveAI() {
            const moves = game.moves({ verbose: true });
            if (moves.length === 0) return null;

            if (aiDifficulty === 'easy') {
                return moves[Math.floor(Math.random() * moves.length)];
            }

            const depth = aiDifficulty === 'med' ? 2 : 3;
            let bestMove = null;
            let bestValue = 99999;

            for (let i = 0; i < moves.length; i++) {
                const move = moves[i];
                game.move(move);
                const value = minimax(depth - 1, game, true, -100000, 100000);
                game.undo();
                if (value < bestValue) {
                    bestValue = value;
                    bestMove = move;
                }
            }
            return bestMove || moves[0];
        }

        // ==========================================
        // 4. PEZZI DARK FANTASY (TESCHI E INTRECCI GOTICI)
        // ==========================================
        function createSkullMesh() {
            const skullGroup = new THREE.Group();
            const cranium = new THREE.Mesh(new THREE.SphereGeometry(0.2, 12, 12), new THREE.MeshStandardMaterial({ roughness: 0.4 }));
            cranium.scale.set(1, 1.1, 1.2);
            skullGroup.add(cranium);

            const jaw = new THREE.Mesh(new THREE.BoxGeometry(0.18, 0.12, 0.18), new THREE.MeshStandardMaterial({ roughness: 0.5 }));
            jaw.position.set(0, -0.12, 0.05);
            skullGroup.add(jaw);

            const eyeMat = new THREE.MeshBasicMaterial({ color: 0x000000 });
            const leftEye = new THREE.Mesh(new THREE.SphereGeometry(0.05, 8, 8), eyeMat);
            leftEye.position.set(-0.07, 0.02, 0.16);
            const rightEye = new THREE.Mesh(new THREE.SphereGeometry(0.05, 8, 8), eyeMat);
            rightEye.position.set(0.07, 0.02, 0.16);
            skullGroup.add(leftEye, rightEye);

            return skullGroup;
        }

        function createGothicGnarledBase(radius, height) {
            const baseGroup = new THREE.Group();
            const ring = new THREE.Mesh(new THREE.CylinderGeometry(radius, radius * 1.15, height, 16), new THREE.MeshStandardMaterial({ roughness: 0.6 }));
            ring.position.y = height / 2;
            baseGroup.add(ring);

            for (let i = 0; i < 8; i++) {
                const angle = (i / 8) * Math.PI * 2;
                const vine = new THREE.Mesh(new THREE.TorusGeometry(radius * 0.7, 0.03, 6, 12, Math.PI), new THREE.MeshStandardMaterial({ roughness: 0.5 }));
                vine.rotation.x = Math.PI / 2 + 0.3;
                vine.rotation.y = angle;
                vine.position.set(Math.cos(angle) * radius * 0.3, height + 0.2, Math.sin(angle) * radius * 0.3);
                baseGroup.add(vine);
            }
            return baseGroup;
        }

        function createPawnGeometry() {
            const group = new THREE.Group();
            group.add(createGothicGnarledBase(0.32, 0.15));
            const stem = new THREE.Mesh(new THREE.ConeGeometry(0.2, 0.7, 8));
            stem.position.y = 0.5;
            group.add(stem);
            const skull = createSkullMesh();
            skull.position.y = 0.95;
            skull.scale.set(0.8, 0.8, 0.8);
            group.add(skull);
            for (let i = 0; i < 4; i++) {
                const spike = new THREE.Mesh(new THREE.ConeGeometry(0.03, 0.2, 4));
                const angle = (i / 4) * Math.PI * 2;
                spike.position.set(Math.cos(angle) * 0.12, 1.15, Math.sin(angle) * 0.12);
                spike.rotation.z = Math.cos(angle) * -0.4;
                spike.rotation.x = Math.sin(angle) * 0.4;
                group.add(spike);
            }
            return group;
        }

        function createRookGeometry() {
            const group = new THREE.Group();
            group.add(createGothicGnarledBase(0.38, 0.18));
            const tower = new THREE.Mesh(new THREE.CylinderGeometry(0.28, 0.34, 0.8, 12));
            tower.position.y = 0.55;
            group.add(tower);
            for (let i = 0; i < 3; i++) {
                const ring = new THREE.Mesh(new THREE.TorusGeometry(0.3, 0.04, 6, 16));
                ring.rotation.x = Math.PI / 2 + 0.2;
                ring.position.y = 0.3 + i * 0.25;
                group.add(ring);
            }
            const crown = new THREE.Mesh(new THREE.CylinderGeometry(0.36, 0.28, 0.25, 8));
            crown.position.y = 1.05;
            group.add(crown);
            return group;
        }

        function createKnightGeometry() {
            const group = new THREE.Group();
            group.add(createGothicGnarledBase(0.38, 0.18));
            const neck = new THREE.Mesh(new THREE.TorusGeometry(0.32, 0.12, 8, 16, Math.PI * 0.85));
            neck.rotation.z = -Math.PI / 3;
            neck.position.set(-0.08, 0.6, 0);
            group.add(neck);
            const head = new THREE.Mesh(new THREE.ConeGeometry(0.16, 0.5, 6));
            head.rotation.z = Math.PI / 2 + 0.3;
            head.position.set(0.12, 0.88, 0);
            group.add(head);
            for (let i = 0; i < 5; i++) {
                const horn = new THREE.Mesh(new THREE.ConeGeometry(0.04, 0.25, 4));
                horn.position.set(-0.15 + i * 0.05, 0.5 + i * 0.1, 0);
                horn.rotation.z = -0.5 - i * 0.1;
                group.add(horn);
            }
            return group;
        }

        function createBishopGeometry() {
            const group = new THREE.Group();
            group.add(createGothicGnarledBase(0.38, 0.18));
            const stem = new THREE.Mesh(new THREE.CylinderGeometry(0.22, 0.3, 0.6, 8));
            stem.position.y = 0.48;
            group.add(stem);
            const skull = createSkullMesh();
            skull.position.y = 0.9;
            skull.scale.set(1.1, 1.1, 1.1);
            group.add(skull);
            const mitre = new THREE.Mesh(new THREE.ConeGeometry(0.24, 0.6, 6));
            mitre.position.y = 1.35;
            mitre.scale.set(1, 1, 0.7);
            group.add(mitre);
            return group;
        }

        function createQueenGeometry() {
            const group = new THREE.Group();
            group.add(createGothicGnarledBase(0.42, 0.2));
            const body = new THREE.Mesh(new THREE.CylinderGeometry(0.18, 0.35, 1.0, 12));
            body.position.y = 0.7;
            group.add(body);
            for (let i = 0; i < 5; i++) {
                const spiral = new THREE.Mesh(new THREE.TorusGeometry(0.24 - i * 0.02, 0.035, 6, 12));
                spiral.rotation.x = Math.PI / 2 + 0.3;
                spiral.position.y = 0.35 + i * 0.18;
                group.add(spiral);
            }
            const skull = createSkullMesh();
            skull.position.y = 1.32;
            skull.scale.set(0.9, 0.9, 0.9);
            group.add(skull);
            for (let i = 0; i < 6; i++) {
                const spike = new THREE.Mesh(new THREE.ConeGeometry(0.04, 0.3, 4));
                const angle = (i / 6) * Math.PI * 2;
                spike.position.set(Math.cos(angle) * 0.18, 1.6, Math.sin(angle) * 0.18);
                spike.rotation.z = Math.cos(angle) * -0.3;
                spike.rotation.x = Math.sin(angle) * 0.3;
                group.add(spike);
            }
            return group;
        }

        function createKingGeometry() {
            const group = new THREE.Group();
            group.add(createGothicGnarledBase(0.45, 0.2));
            const body = new THREE.Mesh(new THREE.CylinderGeometry(0.22, 0.38, 1.1, 12));
            body.position.y = 0.75;
            group.add(body);
            for (let i = 0; i < 6; i++) {
                const spiral = new THREE.Mesh(new THREE.TorusGeometry(0.28 - i * 0.02, 0.04, 6, 12));
                spiral.rotation.x = Math.PI / 2 + 0.2;
                spiral.position.y = 0.35 + i * 0.18;
                group.add(spiral);
            }
            const skull = createSkullMesh();
            skull.position.y = 1.45;
            skull.scale.set(1.15, 1.15, 1.15);
            group.add(skull);
            [-1, 1].forEach(side => {
                const hornBase = new THREE.Mesh(new THREE.TorusGeometry(0.2, 0.05, 6, 12, Math.PI * 0.7));
                hornBase.position.set(side * 0.18, 1.65, 0);
                hornBase.rotation.z = side * -0.8;
                hornBase.rotation.y = side * 0.3;
                group.add(hornBase);
                const hornTip = new THREE.Mesh(new THREE.ConeGeometry(0.05, 0.35, 4));
                hornTip.position.set(side * 0.32, 1.85, 0.05);
                hornTip.rotation.z = side * -0.5;
                group.add(hornTip);
            });
            const crossV = new THREE.Mesh(new THREE.BoxGeometry(0.04, 0.3, 0.04));
            crossV.position.y = 1.95;
            const crossH = new THREE.Mesh(new THREE.BoxGeometry(0.18, 0.04, 0.04));
            crossH.position.y = 2.02;
            group.add(crossV, crossH);
            return group;
        }

        const pieceGeometries = {
            p: createPawnGeometry(),
            r: createRookGeometry(),
            n: createKnightGeometry(),
            b: createBishopGeometry(),
            q: createQueenGeometry(),
            k: createKingGeometry()
        };

        const pieceMaterials = {
            w: new THREE.MeshStandardMaterial({ color: 0xdcd3c1, roughness: 0.4, metalness: 0.2 }),
            b: new THREE.MeshStandardMaterial({ color: 0x121113, roughness: 0.35, metalness: 0.4 })
        };

        // AMBIENTE CATTEDRALE SUPER DETTAGLIATO
        const darkStoneMat = new THREE.MeshStandardMaterial({ color: 0x16141a, roughness: 0.85, metalness: 0.2 });
        const whiteMarbleMat = new THREE.MeshStandardMaterial({ color: 0xede6d6, roughness: 0.3, metalness: 0.1 });
        const blackMarbleMat = new THREE.MeshStandardMaterial({ color: 0x141318, roughness: 0.25, metalness: 0.45 });
        const goldTrimMat = new THREE.MeshStandardMaterial({ color: 0x9a7b4f, roughness: 0.3, metalness: 0.8 });

        const floorGeo = new THREE.PlaneGeometry(120, 120);
        const floor = new THREE.Mesh(floorGeo, darkStoneMat);
        floor.rotation.x = -Math.PI / 2;
        floor.position.y = -2;
        floor.receiveShadow = true;
        scene.add(floor);

        function createRichGothicPillar(x, z) {
            const group = new THREE.Group();
            const base = new THREE.Mesh(new THREE.BoxGeometry(1.8, 1.0, 1.8), darkStoneMat);
            base.position.y = 1.5;
            group.add(base);

            for (let i = 0; i < 4; i++) {
                const angle = (i * Math.PI) / 2;
                const shaft = new THREE.Mesh(new THREE.CylinderGeometry(0.32, 0.32, 22, 12), darkStoneMat);
                shaft.position.set(Math.cos(angle) * 0.45, 12, Math.sin(angle) * 0.45);
                group.add(shaft);
            }

            const capital = new THREE.Mesh(new THREE.BoxGeometry(1.6, 0.6, 1.6), goldTrimMat);
            capital.position.y = 22;
            group.add(capital);

            group.position.set(x, -2, z);
            scene.add(group);
        }

        [-18, 18].forEach(x => {
            [-20, -10, 0, 10, 20].forEach(z => createRichGothicPillar(x, z));
        });

        const ambientLight = new THREE.AmbientLight(0x1e1728, 1.5);
        scene.add(ambientLight);

        const boardLight = new THREE.PointLight(0xffb054, 4.2, 30);
        boardLight.position.set(0, 9, 0);
        boardLight.castShadow = true;
        boardLight.shadow.mapSize.width = 2048;
        boardLight.shadow.mapSize.height = 2048;
        scene.add(boardLight);

        const stainedGlassLight = new THREE.SpotLight(0x3b82f6, 5.0, 60, Math.PI / 3, 0.5);
        stainedGlassLight.position.set(-22, 24, -12);
        stainedGlassLight.target.position.set(0, 0, 0);
        scene.add(stainedGlassLight);
        scene.add(stainedGlassLight.target);

        const animatedCharacters = [];
        function createFlyingGargoyleBat() {
            const group = new THREE.Group();
            const bodyMat = new THREE.MeshStandardMaterial({ color: 0x121015, roughness: 0.9 });
            const body = new THREE.Mesh(new THREE.ConeGeometry(0.25, 0.7, 5), bodyMat);
            body.rotation.x = Math.PI / 2;
            group.add(body);

            const wingGeo = new THREE.BoxGeometry(1.2, 0.05, 0.4);
            const leftWing = new THREE.Mesh(wingGeo, bodyMat);
            leftWing.position.set(0.6, 0, 0);
            group.add(leftWing);

            const rightWing = new THREE.Mesh(wingGeo, bodyMat);
            rightWing.position.set(-0.6, 0, 0);
            group.add(rightWing);

            scene.add(group);
            return {
                mesh: group,
                leftWing,
                rightWing,
                angle: Math.random() * Math.PI * 2,
                radius: 14 + Math.random() * 6,
                speed: 0.007 + Math.random() * 0.01,
                height: 12 + Math.random() * 5
            };
        }

        for (let i = 0; i < 5; i++) animatedCharacters.push(createFlyingGargoyleBat());

        const particleGeo = new THREE.BufferGeometry();
        const particleCount = 350;
        const posArray = new Float32Array(particleCount * 3);
        for (let i = 0; i < particleCount * 3; i += 3) {
            posArray[i] = (Math.random() - 0.5) * 40;
            posArray[i + 1] = Math.random() * 20;
            posArray[i + 2] = (Math.random() - 0.5) * 40;
        }
        particleGeo.setAttribute('position', new THREE.BufferAttribute(posArray, 3));
        const particleMat = new THREE.PointsMaterial({ size: 0.06, color: 0xdfb26d, transparent: true, opacity: 0.65 });
        const particles = new THREE.Points(particleGeo, particleMat);
        scene.add(particles);

        const boardGroup = new THREE.Group();
        scene.add(boardGroup);

        const pedestalBase = new THREE.Mesh(new THREE.BoxGeometry(9.6, 0.6, 9.6), darkStoneMat);
        pedestalBase.position.y = -0.35;
        pedestalBase.receiveShadow = true;
        boardGroup.add(pedestalBase);

        const tileSize = 0.95;
        const boardOffset = (8 * tileSize) / 2 - tileSize / 2;
        const tiles = {};
        const tileGeo = new THREE.BoxGeometry(tileSize * 0.96, 0.15, tileSize * 0.96);

        for (let row = 0; row < 8; row++) {
            for (let col = 0; col < 8; col++) {
                const isDark = (row + col) % 2 === 1;
                const mat = isDark ? blackMarbleMat : whiteMarbleMat;
                const tileMesh = new THREE.Mesh(tileGeo, mat);
                tileMesh.position.set(col * tileSize - boardOffset, 0, row * tileSize - boardOffset);
                tileMesh.receiveShadow = true;

                const square = String.fromCharCode(97 + col) + (8 - row).toString();
                tileMesh.userData = { square };
                boardGroup.add(tileMesh);
                tiles[square] = tileMesh;
            }
        }

        const highlightMats = {
            select: new THREE.MeshBasicMaterial({ color: 0xeab308, transparent: true, opacity: 0.65 }),
            move: new THREE.MeshBasicMaterial({ color: 0x22c55e, transparent: true, opacity: 0.6 }),
            capture: new THREE.MeshBasicMaterial({ color: 0xef4444, transparent: true, opacity: 0.7 })
        };
        const activeHighlights = [];
        let showHighlightsPref = true;

        function clearHighlights() {
            activeHighlights.forEach(h => scene.remove(h));
            activeHighlights.length = 0;
        }

        function addHighlight(square, type) {
            if (!showHighlightsPref) return;
            const tile = tiles[square];
            if (!tile) return;
            const hl = new THREE.Mesh(new THREE.PlaneGeometry(tileSize * 0.94, tileSize * 0.94), highlightMats[type]);
            hl.rotation.x = -Math.PI / 2;
            hl.position.set(tile.position.x, 0.085, tile.position.z);
            scene.add(hl);
            activeHighlights.push(hl);
        }

        const pieceObjects = {};

        function spawnPiece(type, color, square) {
            const mat = pieceMaterials[color];
            const mesh = pieceGeometries[type].clone();
            mesh.traverse(child => {
                if (child.isMesh) {
                    child.material = mat;
                    child.castShadow = document.getElementById('shadow-high').classList.contains('active');
                    child.receiveShadow = true;
                }
            });

            const tile = tiles[square];
            mesh.position.set(tile.position.x, 0.08, tile.position.z);
            if (color === 'b') mesh.rotation.y = Math.PI;

            mesh.userData = { type, color, square };
            scene.add(mesh);
            pieceObjects[square] = mesh;
        }

        function renderBoardState() {
            Object.keys(pieceObjects).forEach(sq => {
                scene.remove(pieceObjects[sq]);
                delete pieceObjects[sq];
            });

            const boardState = game.board();
            for (let row = 0; row < 8; row++) {
                for (let col = 0; col < 8; col++) {
                    const piece = boardState[row][col];
                    if (piece) {
                        spawnPiece(piece.type, piece.color, String.fromCharCode(97 + col) + (8 - row).toString());
                    }
                }
            }
            updateUI();
        }

        function animatePieceMove(fromSquare, toSquare, onComplete) {
            const pieceMesh = pieceObjects[fromSquare];
            if (!pieceMesh) { if (onComplete) onComplete(); return; }

            const targetTile = tiles[toSquare];
            const startPos = pieceMesh.position.clone();
            const endPos = new THREE.Vector3(targetTile.position.x, 0.08, targetTile.position.z);
            let progress = 0;
            const duration = 18;

            function animate() {
                progress++;
                const t = progress / duration;
                pieceMesh.position.lerpVectors(startPos, endPos, t);
                pieceMesh.position.y = 0.08 + Math.sin(t * Math.PI) * 0.7;
                if (progress < duration) {
                    requestAnimationFrame(animate);
                } else {
                    pieceMesh.position.copy(endPos);
                    if (onComplete) onComplete();
                }
            }
            animate();
        }

        const raycaster = new THREE.Raycaster();
        const mouse = new THREE.Vector2();

        function getSquareFromRaycast(event) {
            const clientX = event.touches ? event.touches[0].clientX : event.clientX;
            const clientY = event.touches ? event.touches[0].clientY : event.clientY;
            mouse.x = (clientX / window.innerWidth) * 2 - 1;
            mouse.y = -(clientY / window.innerHeight) * 2 + 1;
            raycaster.setFromCamera(mouse, camera);
            const intersects = raycaster.intersectObjects(boardGroup.children);
            for (let intersect of intersects) {
                if (intersect.object.userData && intersect.object.userData.square) {
                    return intersect.object.userData.square;
                }
            }
            return null;
        }

        function handleSquareClick(square) {
            if (game.game_over() || isAiThinking) return;

            if (selectedSquare) {
                if (selectedSquare === square) {
                    selectedSquare = null;
                    clearHighlights();
                    return;
                }
                const moveObj = legalMoves.find(m => m.to === square);
                if (moveObj) {
                    executeMove(moveObj);
                    return;
                }
            }

            const piece = game.get(square);
            if (piece && piece.color === game.turn()) {
                selectedSquare = square;
                clearHighlights();
                addHighlight(square, 'select');
                legalMoves = game.moves({ square: square, verbose: true });
                legalMoves.forEach(m => addHighlight(m.to, m.captured ? 'capture' : 'move'));
            } else {
                selectedSquare = null;
                clearHighlights();
            }
        }

        let pendingPromotionMove = null;

        function executeMove(moveObj) {
            if (moveObj.flags.includes('p') || (moveObj.piece === 'p' && (moveObj.to[1] === '8' || moveObj.to[1] === '1'))) {
                pendingPromotionMove = moveObj;
                document.getElementById('promotion-modal').classList.remove('hidden');
                return;
            }

            const fromSq = moveObj.from;
            const toSq = moveObj.to;
            const isCapture = !!moveObj.captured;

            clearHighlights();
            selectedSquare = null;

            animatePieceMove(fromSq, toSq, () => {
                const result = game.move(moveObj);
                if (result) {
                    isCapture ? audio.playCapture() : audio.playMove();
                    if (game.in_check()) {
                        audio.playCheck();
                        document.getElementById('status-card').classList.add('in-check-alert');
                    } else {
                        document.getElementById('status-card').classList.remove('in-check-alert');
                    }
                    renderBoardState();

                    if (isCinemaMode && !game.game_over()) {
                        const nextTurnPreset = game.turn() === 'w' ? 'white' : 'black';
                        setCameraPreset(nextTurnPreset);
                    }

                    if (game.game_over()) {
                        handleGameOverCondition();
                    } else if (gameMode === 'ai' && game.turn() === 'b') {
                        triggerAiTurn();
                    }
                }
            });
        }

        function triggerAiTurn() {
            isAiThinking = true;
            document.getElementById('game-status').innerText = "IA in riflessione...";
            setTimeout(() => {
                const aiMove = getBestMoveAI();
                isAiThinking = false;
                if (aiMove) executeMove(aiMove);
            }, 600);
        }

        function handleGameOverCondition() {
            if (timerInterval) clearInterval(timerInterval);
            let title = "Partita Terminata";
            let desc = "La sacra tenuta è giunta a conclusione.";
            if (game.in_checkmate()) {
                const winner = game.turn() === 'w' ? "Nero" : "Bianco";
                title = "Scacco Matto!";
                desc = `Il ${winner} trionfa nella cattedrale gotica!`;
                if (winner === "Bianco" && typeof confetti === 'function') {
                    confetti({ particleCount: 120, spread: 70, origin: { y: 0.6 } });
                }
            } else if (game.in_draw()) {
                title = "Patta / Stallo";
                desc = "Le forze divine si equivalgono. Pareggio!";
            }
            triggerGameOver(title, desc);
        }

        function triggerGameOver(title, desc) {
            document.getElementById('gameover-title').innerText = title;
            document.getElementById('gameover-desc').innerText = desc;
            document.getElementById('gameover-modal').classList.remove('hidden');
        }

        window.addEventListener('pointerdown', (e) => {
            if (e.target.closest('.pointer-events-auto')) return;
            const square = getSquareFromRaycast(e);
            if (square) handleSquareClick(square);
        });

        const pieceIcons = { p: '♟', r: '♜', n: '♞', b: '♝', q: '♛', k: '♚' };

        function updateUI() {
            const turn = game.turn() === 'w' ? "Bianco" : "Nero";
            const statusEl = document.getElementById('game-status');
            const checkEl = document.getElementById('check-status');

            if (game.in_checkmate()) {
                statusEl.innerText = `Matto! ${game.turn() === 'w' ? 'Nero' : 'Bianco'} Vince!`;
                statusEl.className = "font-bold text-red-500";
            } else if (game.in_draw()) {
                statusEl.innerText = "Patta";
                statusEl.className = "font-bold text-amber-300";
            } else {
                statusEl.innerText = `${turn}`;
                statusEl.className = "font-bold text-amber-400";
            }

            if (game.in_check()) {
                checkEl.innerText = "IN SCACCO!";
                checkEl.className = "text-red-500 font-bold animate-pulse";
            } else {
                checkEl.innerText = "Sicuro";
                checkEl.className = "text-emerald-400 font-semibold";
            }

            const history = game.history({ verbose: true });
            const cw = [], cb = [];
            history.forEach(m => {
                if (m.captured) (m.color === 'w' ? cb : cw).push(m.captured);
            });
            document.getElementById('graveyard-white').innerHTML = cw.map(p => pieceIcons[p]).join(' ');
            document.getElementById('graveyard-black').innerHTML = cb.map(p => pieceIcons[p]).join(' ');

            const logEl = document.getElementById('move-log');
            const hList = game.history();
            logEl.innerHTML = '';
            for (let i = 0; i < hList.length; i += 2) {
                const row = document.createElement('div');
                row.className = "flex justify-between border-b border-amber-900/20 py-0.5";
                row.innerHTML = `<span class="text-amber-500/60">${Math.floor(i/2)+1}.</span><span>${hList[i]}</span><span class="text-amber-300/60">${hList[i+1]||''}</span>`;
                logEl.appendChild(row);
            }
            logEl.scrollTop = logEl.scrollHeight;
        }

        document.getElementById('btn-mode-pvp').addEventListener('click', () => {
            gameMode = 'pvp';
            document.getElementById('btn-mode-pvp').classList.add('active');
            document.getElementById('btn-mode-ai').classList.remove('active');
            document.getElementById('ai-difficulty-container').classList.add('hidden');
        });

        document.getElementById('btn-mode-ai').addEventListener('click', () => {
            gameMode = 'ai';
            document.getElementById('btn-mode-ai').classList.add('active');
            document.getElementById('btn-mode-pvp').classList.remove('active');
            document.getElementById('ai-difficulty-container').classList.remove('hidden');
            document.getElementById('ai-difficulty-container').classList.add('flex');
            if (game.turn() === 'b' && !game.game_over()) triggerAiTurn();
        });

        ['easy', 'med', 'hard'].forEach(diff => {
            document.getElementById(`ai-${diff}`).addEventListener('click', (e) => {
                aiDifficulty = diff;
                ['easy', 'med', 'hard'].forEach(d => document.getElementById(`ai-${d}`).classList.remove('active'));
                e.target.classList.add('active');
            });
        });

        ['off', '3m', '5m'].forEach(tKey => {
            document.getElementById(`timer-${tKey}`).addEventListener('click', (e) => {
                timerMode = tKey;
                ['off', '3m', '5m'].forEach(k => document.getElementById(`timer-${k}`).classList.remove('active'));
                e.target.classList.add('active');
                
                const disp = document.getElementById('timer-display-container');
                if (tKey === 'off') {
                    disp.classList.add('hidden');
                    disp.classList.remove('flex');
                    if (timerInterval) clearInterval(timerInterval);
                } else {
                    disp.classList.remove('hidden');
                    disp.classList.add('flex');
                    const secs = tKey === '3m' ? 180 : 300;
                    timeWhite = secs;
                    timeBlack = secs;
                    updateTimerUI();
                    startTimerInterval();
                }
            });
        });

        document.getElementById('btn-hint').addEventListener('click', () => {
            if (game.game_over() || isAiThinking) return;
            const hintMove = getBestMoveAI();
            if (hintMove) {
                clearHighlights();
                addHighlight(hintMove.from, 'select');
                addHighlight(hintMove.to, 'move');
                setTimeout(() => clearHighlights(), 3000);
            }
        });

        document.getElementById('btn-undo').addEventListener('click', () => {
            if (isAiThinking) return;
            game.undo();
            if (gameMode === 'ai' && game.turn() === 'b') game.undo();
            selectedSquare = null;
            clearHighlights();
            document.getElementById('status-card').classList.remove('in-check-alert');
            renderBoardState();
            if (isCinemaMode) {
                setCameraPreset(game.turn() === 'w' ? 'white' : 'black');
            }
        });

        document.getElementById('btn-reset').addEventListener('click', () => {
            game.reset();
            selectedSquare = null;
            clearHighlights();
            isCinemaMode = false;
            document.getElementById('status-card').classList.remove('in-check-alert');
            document.getElementById('gameover-modal').classList.add('hidden');
            if (timerMode !== 'off') {
                const secs = timerMode === '3m' ? 180 : 300;
                timeWhite = secs;
                timeBlack = secs;
                updateTimerUI();
                startTimerInterval();
            }
            setCameraPreset('white', true);
            renderBoardState();
        });

        document.getElementById('btn-play-again').addEventListener('click', () => {
            document.getElementById('btn-reset').click();
        });

        document.getElementById('btn-sound').addEventListener('click', () => {
            audio.soundEnabled = !audio.soundEnabled;
            const icon = document.getElementById('sound-icon');
            icon.className = audio.soundEnabled ? "fa-solid fa-volume-high" : "fa-solid fa-volume-xmark text-red-400";
        });

        document.getElementById('btn-ambient').addEventListener('click', () => {
            const active = audio.toggleAmbient();
            const icon = document.getElementById('ambient-icon');
            icon.className = active ? "fa-solid fa-music text-amber-400 animate-pulse" : "fa-solid fa-music";
        });

        ['white', 'black', 'top', 'cinematic'].forEach(key => {
            document.getElementById(`cam-${key}`).addEventListener('click', (e) => {
                ['white', 'black', 'top', 'cinematic'].forEach(k => document.getElementById(`cam-${k}`).classList.remove('active'));
                e.target.classList.add('active');
                if (key === 'cinematic') {
                    isCinemaMode = true;
                    setCameraPreset(game.turn() === 'w' ? 'white' : 'black');
                } else {
                    isCinemaMode = false;
                    setCameraPreset(key);
                }
            });
        });

        document.querySelectorAll('.promo-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                const pieceType = e.currentTarget.getAttribute('data-piece');
                if (pendingPromotionMove) {
                    pendingPromotionMove.promotion = pieceType;
                    const promoMove = pendingPromotionMove;
                    pendingPromotionMove = null;
                    document.getElementById('promotion-modal').classList.add('hidden');
                    executeMove(promoMove);
                }
            });
        });

        document.getElementById('btn-settings-modal').addEventListener('click', () => {
            document.getElementById('settings-modal').classList.remove('hidden');
        });
        document.getElementById('close-settings').addEventListener('click', () => {
            document.getElementById('settings-modal').classList.add('hidden');
        });

        document.querySelectorAll('.theme-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                document.querySelectorAll('.theme-btn').forEach(b => b.classList.remove('active'));
                e.currentTarget.classList.add('active');
                const theme = e.currentTarget.getAttribute('data-theme');
                if (theme === 'sanctum') {
                    scene.background.setHex(0x030305);
                    boardLight.color.setHex(0xffb054);
                    stainedGlassLight.color.setHex(0x3b82f6);
                } else if (theme === 'crypt') {
                    scene.background.setHex(0x080202);
                    boardLight.color.setHex(0xef4444);
                    stainedGlassLight.color.setHex(0x9333ea);
                } else if (theme === 'moonlight') {
                    scene.background.setHex(0x020408);
                    boardLight.color.setHex(0x38bdf8);
                    stainedGlassLight.color.setHex(0x6366f1);
                }
            });
        });

        document.getElementById('shadow-high').addEventListener('click', () => {
            document.getElementById('shadow-high').classList.add('active');
            document.getElementById('shadow-off').classList.remove('active');
            renderer.shadowMap.enabled = true;
            boardLight.castShadow = true;
            renderBoardState();
        });
        document.getElementById('shadow-off').addEventListener('click', () => {
            document.getElementById('shadow-off').classList.add('active');
            document.getElementById('shadow-high').classList.remove('active');
            renderer.shadowMap.enabled = false;
            boardLight.castShadow = false;
            renderBoardState();
        });

        document.getElementById('toggle-highlights').addEventListener('change', (e) => {
            showHighlightsPref = e.target.checked;
        });

        document.getElementById('btn-mobile-menu').addEventListener('click', () => {
            const panel = document.getElementById('control-panel');
            panel.classList.toggle('hidden');
            panel.classList.toggle('flex');
        });

        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });

        renderBoardState();

        let time = 0;
        function animateLoop() {
            requestAnimationFrame(animateLoop);
            time += 0.01;

            controls.update();

            animatedCharacters.forEach(bat => {
                bat.angle += bat.speed;
                bat.mesh.position.x = Math.cos(bat.angle) * bat.radius;
                bat.mesh.position.z = Math.sin(bat.angle) * bat.radius;
                bat.mesh.position.y = bat.height + Math.sin(time * 2) * 0.6;
                bat.mesh.rotation.y = -bat.angle + Math.PI / 2;
                bat.leftWing.rotation.z = Math.sin(time * 12) * 0.35;
                bat.rightWing.rotation.z = -Math.sin(time * 12) * 0.35;
            });

            const positions = particles.geometry.attributes.position.array;
            for (let i = 1; i < particleCount * 3; i += 3) {
                positions[i] -= 0.012;
                if (positions[i] < 0) positions[i] = 20;
            }
            particles.geometry.attributes.position.needsUpdate = true;

            renderer.render(scene, camera);
        }

        animateLoop();
    </script>
</body>
</html>
