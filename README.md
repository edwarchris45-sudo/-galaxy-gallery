# -galaxy-gallery
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="theme-color" content="#000000">
<title>Galaxy Gallery 💗</title>

<style>
* { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }

html, body {
    width: 100%;
    height: 100%;
    overflow: hidden;
    background: #000;
    color: #fff;
    font-family: Georgia, "Times New Roman", serif;
    overscroll-behavior: none;
    -webkit-user-select: none;
    user-select: none;
    -webkit-touch-callout: none;
}

#space {
    position: fixed;
    inset: 0;
    width: 100%;
    height: 100%;
    display: block;
    touch-action: none;
}

/* ---------- Botones redondos ---------- */
.bar {
    position: fixed;
    top: calc(env(safe-area-inset-top, 0px) + 12px);
    z-index: 8;
    display: flex;
    gap: 10px;
}
.bar.left { left: calc(env(safe-area-inset-left, 0px) + 12px); }
.bar.right { right: calc(env(safe-area-inset-right, 0px) + 12px); }

.round {
    width: 44px;
    height: 44px;
    border-radius: 50%;
    border: 1px solid #ffffff30;
    color: #fff;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    touch-action: manipulation;
    -webkit-appearance: none;
    appearance: none;
    box-shadow: 0 0 18px #0008;
}
#fsBtn { background: linear-gradient(135deg, #8a4fc9, #4f4fb8); }
#editBtn { background: linear-gradient(135deg, #c04fa0, #8a4fc9); }
#musicBtn { background: linear-gradient(135deg, #2f8fa8, #4f4fb8); display: none; }
#musicBtn.off { opacity: .55; }
#pauseBtn {
    background: #e8294f;
    border-color: #ff7a95;
    opacity: 0;
    pointer-events: none;
    transition: opacity 1.2s;
}
#pauseBtn.show { opacity: 1; pointer-events: auto; }

.round svg { width: 20px; height: 20px; fill: none; stroke: #fff; stroke-width: 2; stroke-linecap: round; stroke-linejoin: round; }
#pauseBtn svg { fill: #fff; stroke: none; }

/* ---------- Pantalla de inicio ---------- */
.intro {
    position: fixed;
    inset: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 7;
    padding: env(safe-area-inset-top, 0px) env(safe-area-inset-right, 0px) env(safe-area-inset-bottom, 0px) env(safe-area-inset-left, 0px);
    background:
        radial-gradient(ellipse at 85% 10%, #1b2260 0%, transparent 45%),
        radial-gradient(ellipse at 12% 92%, #4a1244 0%, transparent 48%),
        #07040f;
    transition: opacity 1.1s, visibility 1.1s;
}
.intro.hidden { opacity: 0; visibility: hidden; pointer-events: none; }

.card {
    width: 88%;
    max-width: 440px;
    padding: 34px 24px 30px;
    text-align: center;
    border: 1px solid #ffffff1c;
    border-radius: 20px;
    background: rgba(28, 16, 42, .72);
    box-shadow: 0 25px 70px #000a, 0 0 40px #8a3cff1c;
    -webkit-backdrop-filter: blur(12px);
    backdrop-filter: blur(12px);
}

.card h1 {
    font-size: clamp(34px, 9vw, 52px);
    font-weight: 700;
    color: #dcd6e6;
    margin-bottom: 8px;
    text-shadow: 0 0 14px #ffffff40, 0 0 34px #b28cff55;
}

.bears { width: clamp(150px, 42vw, 210px); margin: 6px auto 4px; display: block; overflow: visible; }
.bears .heart { animation: floatHeart 2.4s ease-in-out infinite; transform-box: fill-box; transform-origin: center; }
.bears .heart.h2 { animation-delay: 1.1s; }
.bears .pair { animation: sway 3s ease-in-out infinite; transform-box: fill-box; transform-origin: 50% 100%; }
@keyframes floatHeart { 0%,100% { transform: translateY(0) scale(1); opacity: 1; } 50% { transform: translateY(-5px) scale(1.12); opacity: .85; } }
@keyframes sway { 0%,100% { transform: rotate(-1.5deg); } 50% { transform: rotate(1.5deg); } }

.subtitle {
    font-family: "Courier New", Courier, monospace;
    font-size: 11px;
    letter-spacing: 3px;
    margin: 6px 0 22px;
    color: #e6dcf2;
}

.startBtn {
    min-height: 46px;
    padding: 0 34px;
    border-radius: 30px;
    border: 1px solid #b784ff70;
    color: #fff;
    font-family: "Courier New", Courier, monospace;
    font-weight: 700;
    font-size: 12px;
    letter-spacing: 3px;
    background: linear-gradient(135deg, #6a3aa8aa, #8d3a9aaa);
    box-shadow: 0 0 22px #a24dff55, inset 0 0 12px #ffffff18;
    cursor: pointer;
    touch-action: manipulation;
    -webkit-appearance: none;
    appearance: none;
    transition: transform .25s;
}
.startBtn:active { transform: scale(1.06); }
@media (hover: hover) { .startBtn:hover { transform: scale(1.07); } }

.hint {
    position: fixed;
    bottom: calc(env(safe-area-inset-bottom, 0px) + 16px);
    left: 10px;
    right: 10px;
    z-index: 5;
    text-align: center;
    color: #f0dcffa0;
    font-family: "Courier New", Courier, monospace;
    font-size: 11px;
    letter-spacing: 1px;
    pointer-events: none;
    opacity: 0;
    transition: opacity 1.5s;
}
.hint.show { opacity: 1; }

@media (max-height: 430px) {
    .card { padding: 14px; }
    .card h1 { font-size: 28px; margin-bottom: 2px; }
    .bears { width: 90px; }
    .subtitle { margin: 2px 0 10px; }
}

/* ---------- Panel de edición ---------- */
input, textarea { -webkit-user-select: text; user-select: text; }

.panel {
    position: fixed; inset: 0; z-index: 30;
    display: none;
    background: rgba(6, 3, 14, .94);
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
    overscroll-behavior: contain;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Arial, sans-serif;
}
.panel.open { display: block; }
.sheet {
    max-width: 540px; margin: 0 auto;
    padding: calc(env(safe-area-inset-top, 0px) + 14px) 16px calc(env(safe-area-inset-bottom, 0px) + 110px);
}
.phead { display: flex; align-items: center; justify-content: space-between; margin-bottom: 14px; }
.phead h2 { font-size: 22px; color: #f3dcff; font-weight: 700; }
.xbtn {
    width: 40px; height: 40px; border-radius: 50%;
    border: 1px solid #ffffff30; background: #ffffff12; color: #fff;
    font-size: 18px; cursor: pointer; touch-action: manipulation;
}
.sec {
    background: #ffffff0d; border: 1px solid #ffffff18; border-radius: 16px;
    padding: 14px; margin-bottom: 14px;
}
.sec h3 { font-size: 15px; color: #ffb6e6; margin-bottom: 10px; letter-spacing: .5px; }
.sec label.f { display: block; font-size: 12px; color: #cdb8e6; margin: 10px 0 5px; }
.sec input[type=text], .sec input[type=password], .sec textarea, .lbox input {
    width: 100%; padding: 11px 12px; border-radius: 10px;
    border: 1px solid #ffffff25; background: #0d0718; color: #fff;
    font-size: 16px; font-family: inherit; outline: none;
}
.sec input[type=text]:focus, .sec input[type=password]:focus, .sec textarea:focus, .lbox input:focus { border-color: #c36bff; }
.sec textarea { resize: vertical; min-height: 120px; line-height: 1.5; }
.note { font-size: 12px; color: #a996c4; margin-top: 8px; line-height: 1.45; }
.vh { position: absolute; width: 1px; height: 1px; opacity: 0; overflow: hidden; pointer-events: none; }
.pbtn {
    display: inline-flex; align-items: center; justify-content: center;
    min-height: 44px; padding: 0 16px; border-radius: 12px;
    border: 1px solid #b784ff70; background: #6a3aa855; color: #fff;
    font-size: 14px; font-weight: 600; cursor: pointer; touch-action: manipulation;
    -webkit-appearance: none; appearance: none; font-family: inherit;
}
.pbtn.ghost { background: transparent; border-color: #ffffff30; color: #e6d6f5; }
.pbtn.danger { border-color: #ff6a8a80; color: #ffb3c2; background: transparent; }
.row { display: flex; gap: 10px; flex-wrap: wrap; align-items: center; }
.grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(72px, 1fr)); gap: 10px; margin-bottom: 12px; }
.thumb { position: relative; aspect-ratio: 1; border-radius: 50%; overflow: visible; }
.thumb img { width: 100%; height: 100%; border-radius: 50%; object-fit: cover; border: 2px solid #ffffffb0; display: block; }
.thumb button {
    position: absolute; top: -4px; right: -4px; width: 26px; height: 26px; border-radius: 50%;
    border: none; background: #e8294f; color: #fff; font-size: 14px; line-height: 1; cursor: pointer; touch-action: manipulation;
}
.coverPrev { max-width: 120px; max-height: 100px; border-radius: 10px; display: block; margin-bottom: 10px; }
.mname { font-size: 14px; color: #f0e4ff; margin-bottom: 10px; word-break: break-all; }
.pfoot {
    position: fixed; left: 0; right: 0; bottom: 0; z-index: 31; display: none;
    padding: 12px 16px calc(env(safe-area-inset-bottom, 0px) + 12px);
    background: linear-gradient(to top, #06030e 70%, transparent);
}
.panel.open ~ .pfoot { display: block; }
.pfoot .in { max-width: 540px; margin: 0 auto; display: flex; gap: 10px; }
.pfoot .pbtn { flex: 1; }
.pfoot .save { background: linear-gradient(135deg, #8a3acb, #e13eaa); border-color: transparent; }

.toast {
    position: fixed; left: 50%; bottom: calc(env(safe-area-inset-bottom, 0px) + 90px);
    transform: translateX(-50%); z-index: 40;
    background: #1d1030f0; border: 1px solid #b784ff60; color: #fff;
    padding: 10px 18px; border-radius: 22px; font-size: 14px;
    font-family: -apple-system, BlinkMacSystemFont, Arial, sans-serif;
    opacity: 0; pointer-events: none; transition: opacity .3s; max-width: 90%; text-align: center;
}
.toast.show { opacity: 1; }

.bears.cover { width: auto; max-width: clamp(150px, 50vw, 240px); max-height: 150px; height: auto; border-radius: 12px; object-fit: contain; }

.lock { position: fixed; inset: 0; z-index: 35; display: none; align-items: center; justify-content: center;
        background: rgba(4, 2, 10, .9); padding: 20px; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Arial, sans-serif; }
.lock.open { display: flex; }
.lbox { width: 100%; max-width: 340px; background: #150b24; border: 1px solid #ffffff22; border-radius: 18px; padding: 22px 18px; text-align: center; }
.lbox h3 { font-size: 20px; color: #f3dcff; margin-bottom: 14px; }
.lbox input { width: 100%; padding: 12px; border-radius: 10px; border: 1px solid #ffffff25; background: #0d0718; color: #fff; font-size: 16px; text-align: center; outline: none; margin-bottom: 12px; }
.lbox .row { justify-content: center; }
.lbox .pbtn { flex: 1; }
.lbox .save { background: linear-gradient(135deg, #8a3acb, #e13eaa); border-color: transparent; }
.lbox .note { color: #ff9db3; min-height: 16px; }
</style>
</head>

<body>

<canvas id="space"></canvas>

<div class="bar left">
    <button class="round" id="fsBtn" aria-label="Pantalla completa" style="display:none">
        <svg viewBox="0 0 24 24"><path d="M4 9V4h5M20 9V4h-5M4 15v5h5M20 15v5h-5"/></svg>
    </button>
    <button class="round" id="editBtn" aria-label="Personalizar">
        <svg viewBox="0 0 24 24"><path d="M12 20h9M16.5 3.5a2.1 2.1 0 013 3L7 19l-4 1 1-4z"/></svg>
    </button>
</div>

<div class="bar right">
    <button class="round" id="musicBtn" aria-label="Música">
        <svg viewBox="0 0 24 24"><path d="M9 18V6l11-2v12"/><circle cx="6.5" cy="18" r="2.5"/><circle cx="17.5" cy="16" r="2.5"/></svg>
    </button>
    <button class="round" id="pauseBtn" aria-label="Pausar">
        <svg id="pauseIcon" viewBox="0 0 24 24"><rect x="6" y="5" width="4" height="14" rx="1"/><rect x="14" y="5" width="4" height="14" rx="1"/></svg>
    </button>
</div>

<section class="intro" id="intro">
    <div class="card">
        <h1 id="title">Galaxy Gallery</h1>

        <!-- Osos abrazados (dibujados en SVG, no necesitan archivos) -->
        <img class="bears cover" id="coverImg" alt="" style="display:none">
        <svg class="bears" id="bearsSvg" viewBox="0 0 170 130" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
            <path class="heart" d="M70 22c-4-8-16-4-12 6 2 5 12 11 12 11s10-6 12-11c4-10-8-14-12-6z" fill="#ff7aa8"/>
            <path class="heart h2" d="M104 14c-3-6-12-3-9 4 1 4 9 8 9 8s8-4 9-8c3-7-6-10-9-4z" fill="#ff5f95" transform="scale(.85) translate(22 6)"/>
            <g class="pair">
                <!-- oso marrón -->
                <ellipse cx="116" cy="102" rx="24" ry="25" fill="#c58b57"/>
                <circle cx="96" cy="58" r="12" fill="#c58b57" opacity="0"/>
                <circle cx="102" cy="62" r="9" fill="#b37845"/>
                <circle cx="136" cy="62" r="9" fill="#b37845"/>
                <circle cx="119" cy="76" r="24" fill="#c58b57"/>
                <ellipse cx="119" cy="84" rx="10" ry="8" fill="#e8c9a0"/>
                <circle cx="111" cy="73" r="2.4" fill="#3a2418"/>
                <circle cx="127" cy="73" r="2.4" fill="#3a2418"/>
                <ellipse cx="119" cy="81" rx="3" ry="2" fill="#3a2418"/>
                <circle cx="130" cy="82" r="2.2" fill="#52d6d0"/>
                <!-- oso blanco -->
                <ellipse cx="62" cy="104" rx="25" ry="24" fill="#fff" stroke="#e9dfe8" stroke-width="1.5"/>
                <circle cx="42" cy="64" r="9" fill="#fff" stroke="#e9dfe8" stroke-width="1.5"/>
                <circle cx="42" cy="64" r="4.5" fill="#ffc7d8"/>
                <circle cx="78" cy="64" r="9" fill="#fff" stroke="#e9dfe8" stroke-width="1.5"/>
                <circle cx="78" cy="64" r="4.5" fill="#ffc7d8"/>
                <circle cx="60" cy="78" r="24" fill="#fff" stroke="#e9dfe8" stroke-width="1.5"/>
                <path d="M47 75q4-5 8 0M64 75q4-5 8 0" stroke="#3a2a30" stroke-width="2.4" fill="none" stroke-linecap="round"/>
                <path d="M54 83q6 9 12 0z" fill="#ff7a8e"/>
                <circle cx="46" cy="83" r="4.5" fill="#ffb3c6" opacity=".8"/>
                <circle cx="74" cy="83" r="4.5" fill="#ffb3c6" opacity=".8"/>
                <!-- abrazo -->
                <path d="M80 100q18-8 34 0" stroke="#fff" stroke-width="9" fill="none" stroke-linecap="round"/>
            </g>
        </svg>

        <div class="subtitle" id="subtitle">PARA MI PRINCESITA</div>
        <button class="startBtn" id="startBtn">INICIAR</button>
    </div>
</section>


<!-- ================= PANEL PERSONALIZAR ================= -->
<div class="panel" id="panel">
  <div class="sheet">
    <div class="phead"><h2>✎ Personalizar</h2><button class="xbtn" id="closeP" aria-label="Cerrar">✕</button></div>

    <div class="sec">
      <h3>Textos</h3>
      <label class="f" for="inTitle">Título</label>
      <input type="text" id="inTitle" maxlength="30">
      <label class="f" for="inSub">Subtítulo</label>
      <input type="text" id="inSub" maxlength="40">
      <label class="f" for="inPhrases">Frases que giran en la galaxia (una por línea, máximo 8)</label>
      <textarea id="inPhrases" maxlength="400"></textarea>
    </div>

    <div class="sec">
      <h3>Fotos de las burbujas</h3>
      <div class="grid" id="photoGrid"></div>
      <div class="row">
        <label class="pbtn" for="inPhotos">＋ Agregar fotos</label>
        <input class="vh" type="file" id="inPhotos" accept="image/*" multiple>
      </div>
      <p class="note">Hasta 12 fotos. Se recortan en círculo desde el centro. Si no agregas ninguna, se usa el dibujito "te amo".</p>
    </div>

    <div class="sec">
      <h3>Imagen de portada</h3>
      <img class="coverPrev" id="coverPrev" alt="" style="display:none">
      <div class="row">
        <label class="pbtn" for="inCover">Elegir imagen</label>
        <input class="vh" type="file" id="inCover" accept="image/*">
        <button class="pbtn ghost" id="rmCover" type="button">Quitar</button>
      </div>
      <p class="note">Reemplaza a los osos de la pantalla de inicio. Mejor si es PNG con fondo transparente.</p>
    </div>

    <div class="sec">
      <h3>Música</h3>
      <div class="mname" id="musicLabel">Sin música</div>
      <div class="row">
        <label class="pbtn" for="inMusic">Elegir canción</label>
        <input class="vh" type="file" id="inMusic" accept="audio/*,.mp3,.m4a,.wav,.aac,.ogg">
        <button class="pbtn ghost" id="rmMusic" type="button">Quitar</button>
      </div>
      <p class="note">Suena en bucle al pulsar INICIAR. Máximo 20 MB. En iPhone, si no escuchas nada, desactiva el modo silencio (el interruptor lateral).</p>
    </div>

    <div class="sec">
      <h3>🔒 Contraseña de edición</h3>
      <div class="mname" id="lockStatus"></div>
      <label class="f" for="inPass">Nueva contraseña (mínimo 4 caracteres)</label>
      <input type="password" id="inPass" maxlength="40" autocomplete="new-password">
      <div class="row" style="margin-top:10px">
        <button class="pbtn" id="setPass" type="button">Establecer</button>
        <button class="pbtn ghost" id="rmPass" type="button">Quitar contraseña</button>
      </div>
      <p class="note">Evita que otra persona cambie la galaxia desde su dispositivo. Es una protección sencilla. Si la olvidas, borra los datos del sitio en el navegador.</p>
    </div>

    <p class="note" style="text-align:center;margin-top:4px">Todo se guarda en este dispositivo y navegador.</p>
  </div>
</div>
<div class="pfoot"><div class="in">
    <button class="pbtn danger" id="resetP" type="button">Restaurar</button>
    <button class="pbtn save" id="saveP" type="button">Guardar</button>
</div></div>
<div class="toast" id="toast"></div>

<div class="lock" id="lock">
  <div class="lbox">
    <h3>🔒 Editar</h3>
    <input type="password" id="lockIn" placeholder="Contraseña" autocomplete="off">
    <div class="row">
      <button class="pbtn ghost" id="lockCancel" type="button">Cancelar</button>
      <button class="pbtn save" id="lockOk" type="button">Entrar</button>
    </div>
    <p class="note" id="lockErr"></p>
  </div>
</div>

<div class="hint" id="hint">Desliza para explorar · pellizca para acercar ✦</div>

<script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>
<script>
(function () {

/* =====================================================
   ✏️ PERSONALIZA AQUÍ
===================================================== */

const DEFAULTS = {
    title: "Galaxy Gallery",
    subtitle: "PARA MI PRINCESITA",
    phrases: ["MI CORAZÓN ES TUYO", "CONTIGO SIEMPRE", "AMOR ETERNO ✦", "INFINITO ∞"]
};
let cfg = { title: DEFAULTS.title, subtitle: DEFAULTS.subtitle, phrases: DEFAULTS.phrases.slice() };
let photos = [];      // fotos de las burbujas (data URLs)
let cover = null;     // imagen de portada (data URL)


/* =====================================================
   DISPOSITIVO / CALIDAD
===================================================== */
const isTouch = window.matchMedia("(pointer: coarse)").matches || "ontouchstart" in window;
const isSmall = Math.min(window.innerWidth, window.innerHeight) < 700;
const quality = isTouch ? (isSmall ? 0.55 : 0.8) : 1;

const hintEl = document.getElementById("hint");
if (!isTouch) hintEl.textContent = "Arrastra para explorar · rueda para acercar ✦";

/* =====================================================
   ESCENA
===================================================== */
const canvas = document.getElementById("space");
const scene = new THREE.Scene();
scene.background = new THREE.Color(0x000000);

const camera = new THREE.PerspectiveCamera(55, window.innerWidth / window.innerHeight, 0.1, 700);

const renderer = new THREE.WebGLRenderer({ canvas, antialias: !isTouch, powerPreference: "high-performance" });
renderer.outputColorSpace = THREE.SRGBColorSpace;

let dpr = 1;

/* ---------- utilidades de textura ---------- */
function glowTexture(c1, c2) {
    const c = document.createElement("canvas");
    c.width = c.height = 128;
    const x = c.getContext("2d");
    const g = x.createRadialGradient(64, 64, 0, 64, 64, 64);
    g.addColorStop(0, c1);
    g.addColorStop(0.35, c2);
    g.addColorStop(1, "rgba(0,0,0,0)");
    x.fillStyle = g;
    x.fillRect(0, 0, 128, 128);
    const t = new THREE.CanvasTexture(c);
    t.colorSpace = THREE.SRGBColorSpace;
    return t;
}

function dotTexture() {
    const c = document.createElement("canvas");
    c.width = c.height = 32;
    const x = c.getContext("2d");
    const g = x.createRadialGradient(16, 16, 0, 16, 16, 16);
    g.addColorStop(0, "#fff");
    g.addColorStop(0.55, "#fff");
    g.addColorStop(1, "rgba(255,255,255,0)");
    x.fillStyle = g;
    x.fillRect(0, 0, 32, 32);
    return new THREE.CanvasTexture(c);
}
const dotTex = dotTexture();

const rand = Math.random;
const gauss = () => (rand() + rand() + rand() - 1.5) / 1.5;
const clamp01 = v => Math.max(0, Math.min(1, v));
const smooth = v => { v = clamp01(v); return v * v * (3 - 2 * v); };
const ease = v => { v = clamp01(v); return v < 0.5 ? 4 * v * v * v : 1 - Math.pow(-2 * v + 2, 3) / 2; };

/* =====================================================
   GALAXIA ESPIRAL
===================================================== */
const GALAXY_R = 48;
const galaxyCount = Math.round(60000 * quality);
const gPos = new Float32Array(galaxyCount * 3);
const gCol = new Float32Array(galaxyCount * 3);

const stops = [
    [0.00, new THREE.Color("#fff4dc")],
    [0.07, new THREE.Color("#ffd2a0")],
    [0.20, new THREE.Color("#ff9a80")],
    [0.38, new THREE.Color("#ff6fa8")],
    [0.58, new THREE.Color("#b25cff")],
    [1.00, new THREE.Color("#3b2a96")]
];
function galaxyColor(t, out) {
    for (let i = 1; i < stops.length; i++) {
        if (t <= stops[i][0]) {
            const a = stops[i - 1], b = stops[i];
            out.copy(a[1]).lerp(b[1], (t - a[0]) / (b[0] - a[0]));
            return out;
        }
    }
    return out.copy(stops[stops.length - 1][1]);
}

const tmpC = new THREE.Color();
for (let i = 0; i < galaxyCount; i++) {
    const halo = rand() < 0.28; // "suelo" de puntitos que llena el fondo
    const r = halo ? Math.sqrt(rand()) * GALAXY_R * 1.1 + 1.5 : Math.pow(rand(), 1.35) * GALAXY_R + 0.9;
    const t = r / GALAXY_R;
    const arm = rand() < 0.5 ? 0 : 1;
    let ang = arm * Math.PI + r * 0.22;
    ang += halo ? rand() * Math.PI * 2 : gauss() * (0.22 + r * 0.018);

    gPos[i * 3]     = Math.cos(ang) * r;
    gPos[i * 3 + 1] = gauss() * (0.15 + r * 0.012);
    gPos[i * 3 + 2] = Math.sin(ang) * r;

    galaxyColor(Math.min(t, 1), tmpC);
    const b = (halo ? 0.45 : 0.75) * (0.55 + rand() * 0.6) * (1 - t * 0.35);
    gCol[i * 3] = tmpC.r * b;
    gCol[i * 3 + 1] = tmpC.g * b;
    gCol[i * 3 + 2] = tmpC.b * b;
}

const gGeo = new THREE.BufferGeometry();
gGeo.setAttribute("position", new THREE.BufferAttribute(gPos, 3));
gGeo.setAttribute("color", new THREE.BufferAttribute(gCol, 3));
const gMat = new THREE.PointsMaterial({
    size: isTouch ? 0.19 : 0.15,
    vertexColors: true,
    transparent: true,
    opacity: 1,
    depthWrite: false,
    blending: THREE.AdditiveBlending,
    map: dotTex,
    alphaTest: 0.01
});
const galaxyPts = new THREE.Points(gGeo, gMat);
scene.add(galaxyPts);

/* ---------- estrellas lejanas ---------- */
const farCount = Math.round(1800 * quality);
const farPos = new Float32Array(farCount * 3);
for (let i = 0; i < farCount; i++) {
    const v = new THREE.Vector3(gauss(), gauss(), gauss()).normalize().multiplyScalar(150 + rand() * 200);
    farPos.set([v.x, v.y, v.z], i * 3);
}
const farGeo = new THREE.BufferGeometry();
farGeo.setAttribute("position", new THREE.BufferAttribute(farPos, 3));
const farMat = new THREE.PointsMaterial({ color: 0xffffff, size: 1.4, sizeAttenuation: false, transparent: true, opacity: 0.75, map: dotTex, depthWrite: false });
scene.add(new THREE.Points(farGeo, farMat));

/* =====================================================
   AGUJERO NEGRO
===================================================== */
const hole = new THREE.Group();
scene.add(hole);

const holeCore = new THREE.Mesh(new THREE.SphereGeometry(0.95, 32, 24), new THREE.MeshBasicMaterial({ color: 0x000000 }));
holeCore.position.y = 0.35;
hole.add(holeCore);

const disk = new THREE.Mesh(
    new THREE.RingGeometry(1.05, 2.15, 96),
    new THREE.MeshBasicMaterial({ color: 0xffd84a, side: THREE.DoubleSide, transparent: true, opacity: 0.97 })
);
disk.rotation.x = -Math.PI / 2;
disk.position.y = 0.12;
hole.add(disk);

const diskOuter = new THREE.Mesh(
    new THREE.RingGeometry(2.25, 2.6, 96),
    new THREE.MeshBasicMaterial({ color: 0xff8a3a, side: THREE.DoubleSide, transparent: true, opacity: 0.5, blending: THREE.AdditiveBlending, depthWrite: false })
);
diskOuter.rotation.x = -Math.PI / 2;
diskOuter.position.y = 0.1;
hole.add(diskOuter);

const diskGlow = new THREE.Sprite(new THREE.SpriteMaterial({
    map: glowTexture("rgba(255,220,120,.9)", "rgba(255,150,60,.35)"),
    transparent: true, opacity: 0.8, blending: THREE.AdditiveBlending, depthWrite: false
}));
diskGlow.scale.set(11, 11, 1);
diskGlow.position.y = 0.2;
hole.add(diskGlow);

/* =====================================================
   CORAZÓN DE PARTÍCULAS (sobre el agujero negro)
===================================================== */
function heartPoint(t, s) {
    return [
        Math.pow(Math.sin(t), 3) * s,
        (13 * Math.cos(t) - 5 * Math.cos(2 * t) - 2 * Math.cos(3 * t) - Math.cos(4 * t)) / 16 * s
    ];
}

const HEART_S = 6;
const heartCount = Math.round(9000 * quality);
const hPos = new Float32Array(heartCount * 3);
const hCol = new Float32Array(heartCount * 3);
const hc = new THREE.Color();

for (let i = 0; i < heartCount; i++) {
    const t = rand() * Math.PI * 2;
    const [ex, ey] = heartPoint(t, HEART_S);
    const k = rand();
    let inward;
    if (k < 0.56) inward = 0.9 + rand() * 0.1;          // borde grueso
    else if (k < 0.78) inward = 0.6 + rand() * 0.1;     // corazón interior
    else inward = Math.sqrt(rand()) * 0.85;             // relleno suave

    hPos[i * 3]     = ex * inward + gauss() * 0.14;
    hPos[i * 3 + 1] = ey * inward + gauss() * 0.14;
    hPos[i * 3 + 2] = gauss() * 0.35;

    hc.setHSL(0.84 + rand() * 0.06, 1, 0.5 + rand() * 0.22);
    hCol[i * 3] = hc.r; hCol[i * 3 + 1] = hc.g; hCol[i * 3 + 2] = hc.b;
}
const hGeo = new THREE.BufferGeometry();
hGeo.setAttribute("position", new THREE.BufferAttribute(hPos, 3));
hGeo.setAttribute("color", new THREE.BufferAttribute(hCol, 3));
const hMat = new THREE.PointsMaterial({
    size: isTouch ? 0.13 : 0.1,
    vertexColors: true,
    transparent: true,
    opacity: 0,
    depthWrite: false,
    blending: THREE.AdditiveBlending,
    map: dotTex
});
const heartGroup = new THREE.Group();
const heartPts = new THREE.Points(hGeo, hMat);
heartGroup.add(heartPts);
heartGroup.position.y = 1.4 + HEART_S * 1.06;
scene.add(heartGroup);

// brillos: punta del corazón y hendidura superior
const tipGlow = new THREE.Sprite(new THREE.SpriteMaterial({
    map: glowTexture("rgba(255,255,255,1)", "rgba(255,70,200,.55)"),
    transparent: true, opacity: 0, blending: THREE.AdditiveBlending, depthWrite: false
}));
tipGlow.scale.set(3.4, 3.4, 1);
tipGlow.position.set(0, -HEART_S * 1.06, 0);
heartGroup.add(tipGlow);

const notchGlow = new THREE.Sprite(tipGlow.material.clone());
notchGlow.scale.set(2.4, 2.4, 1);
notchGlow.position.set(0, HEART_S * 0.31, 0);
heartGroup.add(notchGlow);

/* =====================================================
   TEXTOS (monoespaciados con brillo rosa)
===================================================== */
function textTexture(text) {
    const c = document.createElement("canvas");
    c.width = 1024; c.height = 128;
    const x = c.getContext("2d");
    const chars = [...text];
    let fs = 54, total = 0, widths = [];
    const measure = () => {
        x.font = "bold " + fs + "px 'Courier New', Courier, monospace";
        total = 0;
        widths = chars.map(ch => { const w = x.measureText(ch).width + 7 * fs / 54; total += w; return w; });
    };
    measure();
    while (total > 980 && fs > 18) { fs -= 2; measure(); }
    x.textBaseline = "middle";
    const px = (c.width - total) / 2;
    [["#ff4fc3", 26, "#ffd6f0"], ["#ff4fc3", 10, "#ffe3f5"]].forEach(([sc, sb, fill]) => {
        x.shadowColor = sc; x.shadowBlur = sb; x.fillStyle = fill;
        let cx = px;
        chars.forEach((ch, i) => { x.fillText(ch, cx, 64); cx += widths[i]; });
    });
    const t = new THREE.CanvasTexture(c);
    t.colorSpace = THREE.SRGBColorSpace;
    t.anisotropy = 4;
    return t;
}

let textTextures = cfg.phrases.map(textTexture);
const textItems = [];
const TEXT_COUNT = 12;
for (let i = 0; i < TEXT_COUNT; i++) {
    const mat = new THREE.MeshBasicMaterial({
        map: textTextures[i % textTextures.length],
        transparent: true, opacity: 0, depthWrite: false,
        blending: THREE.AdditiveBlending, side: THREE.DoubleSide
    });
    const mesh = new THREE.Mesh(new THREE.PlaneGeometry(8, 1), mat);
    scene.add(mesh);
    textItems.push({
        mesh,
        angle: (i / TEXT_COUNT) * Math.PI * 2 + rand() * 0.3,
        radius: 11 + (i % 4) * 4.5 + rand() * 2,
        y: 0.8 + (i % 3) * 1.4 + rand() * 0.6,
        speed: (0.06 + rand() * 0.05) * (i % 2 ? 1 : -1),
        base: 1,
        phase: rand() * 6.28
    });
}

/* =====================================================
   BURBUJAS DE FOTOS
===================================================== */
function drawDoodle(x, s) {
    x.fillStyle = "#f4f4f4";
    x.fillRect(0, 0, s, s);
    x.strokeStyle = "#111"; x.fillStyle = "#fff"; x.lineWidth = s * 0.014; x.lineCap = "round";
    // texto pequeño arriba
    x.fillStyle = "#222"; x.font = (s * 0.052) + "px Arial"; x.textAlign = "center";
    x.fillText("tengo a recordarte", s / 2, s * 0.2);
    x.fillText("por si se te olvida", s / 2, s * 0.26);
    // cabeza
    x.fillStyle = "#fff";
    x.beginPath(); x.ellipse(s * 0.33, s * 0.5, s * 0.1, s * 0.09, 0, 0, 7); x.fill(); x.stroke();
    x.fillStyle = "#111";
    x.beginPath(); x.arc(s * 0.3, s * 0.49, s * 0.011, 0, 7); x.fill();
    x.beginPath(); x.arc(s * 0.37, s * 0.49, s * 0.011, 0, 7); x.fill();
    // cuerpo
    x.fillStyle = "#fff";
    x.beginPath(); x.moveTo(s * 0.27, s * 0.58); x.quadraticCurveTo(s * 0.33, s * 0.56, s * 0.4, s * 0.58);
    x.lineTo(s * 0.41, s * 0.82); x.lineTo(s * 0.26, s * 0.82); x.closePath(); x.fill(); x.stroke();
    // cartel
    x.fillStyle = "#fff";
    x.beginPath(); x.rect(s * 0.34, s * 0.5, s * 0.4, s * 0.24); x.fill(); x.stroke();
    x.fillStyle = "#111"; x.font = "bold " + (s * 0.085) + "px Arial";
    x.fillText("te amo", s * 0.52, s * 0.64);
    x.fillStyle = "#e0245e"; x.font = (s * 0.08) + "px Arial";
    x.fillText("♥️", s * 0.67, s * 0.62);
    // brazos
    x.beginPath(); x.moveTo(s * 0.27, s * 0.62); x.lineTo(s * 0.34, s * 0.66); x.stroke();
    x.beginPath(); x.moveTo(s * 0.41, s * 0.62); x.lineTo(s * 0.45, s * 0.58); x.stroke();
}

function photoTexture(index, onLoad) {
    const s = 384;
    const c = document.createElement("canvas");
    c.width = c.height = s;
    const x = c.getContext("2d");
    const tex = new THREE.CanvasTexture(c);
    tex.colorSpace = THREE.SRGBColorSpace;

    function frame() {
        x.save();
        x.globalCompositeOperation = "destination-in";
        x.beginPath(); x.arc(s / 2, s / 2, s / 2 - 3, 0, 7); x.fill();
        x.restore();
        x.strokeStyle = "rgba(255,255,255,.9)"; x.lineWidth = 5;
        x.beginPath(); x.arc(s / 2, s / 2, s / 2 - 3, 0, 7); x.stroke();
        tex.needsUpdate = true;
    }

    if (photos.length) {
        const img = new Image();
        img.onload = () => {
            const m = Math.min(img.width, img.height);
            x.drawImage(img, (img.width - m) / 2, (img.height - m) / 2, m, m, 0, 0, s, s);
            frame();
        };
        img.src = photos[index % photos.length];
        x.fillStyle = "#eee"; x.fillRect(0, 0, s, s);
        frame();
    } else {
        drawDoodle(x, s);
        frame();
    }
    return tex;
}

const photoItems = [];
function buildPhotos() {
    photoItems.forEach(it => { scene.remove(it.sprite); if (it.sprite.material.map) it.sprite.material.map.dispose(); it.sprite.material.dispose(); });
    photoItems.length = 0;
    const count = photos.length ? Math.max(6, photos.length) : 7;
    for (let i = 0; i < count; i++) {
        const mat = new THREE.SpriteMaterial({ map: photoTexture(i), transparent: true, opacity: 0, depthWrite: false });
        const sp = new THREE.Sprite(mat);
        const size = 3.6 + rand() * 1.8;
        sp.scale.set(size, size, 1);
        scene.add(sp);
        photoItems.push({
            sprite: sp,
            angle: (i / count) * Math.PI * 2 + rand() * 0.4,
            radius: 9 + rand() * 15,
            y: 2.2 + rand() * 4.5,
            speed: (0.05 + rand() * 0.05) * (i % 2 ? 1 : -1),
            phase: rand() * 6.28
        });
    }
}
buildPhotos();

/* =====================================================
   CORAZONES FLOTANTES
===================================================== */
function heartSpriteTexture() {
    const c = document.createElement("canvas");
    c.width = c.height = 128;
    const x = c.getContext("2d");
    x.translate(64, 62);
    x.beginPath();
    for (let a = 0; a <= Math.PI * 2 + 0.05; a += 0.05) {
        const px = 3.3 * 16 * Math.pow(Math.sin(a), 3);
        const py = -3.3 * (13 * Math.cos(a) - 5 * Math.cos(2 * a) - 2 * Math.cos(3 * a) - Math.cos(4 * a));
        a === 0 ? x.moveTo(px, py) : x.lineTo(px, py);
    }
    x.closePath();
    const g = x.createRadialGradient(-10, -14, 4, 0, 0, 60);
    g.addColorStop(0, "#ffd0ea"); g.addColorStop(0.5, "#ff6fb8"); g.addColorStop(1, "#d81e84");
    x.shadowColor = "#ff4fc3"; x.shadowBlur = 24;
    x.fillStyle = g; x.fill();
    const t = new THREE.CanvasTexture(c);
    t.colorSpace = THREE.SRGBColorSpace;
    return t;
}
const heartSprTex = heartSpriteTexture();
const floatHearts = [];
for (let i = 0; i < 6; i++) {
    const sp = new THREE.Sprite(new THREE.SpriteMaterial({ map: heartSprTex, transparent: true, opacity: 0, depthWrite: false }));
    const s = i === 0 ? 9 : 1.2 + rand() * 1.4;
    sp.scale.set(s, s, 1);
    scene.add(sp);
    floatHearts.push({
        sprite: sp, size: s,
        angle: rand() * 6.28,
        radius: i === 0 ? 17 : 6 + rand() * 20,
        y: 2 + rand() * 7,
        speed: (0.04 + rand() * 0.06) * (i % 2 ? 1 : -1),
        phase: rand() * 6.28
    });
}

/* =====================================================
   CONFETI DE INICIO
===================================================== */
const confettiGroups = [];
function makeConfetti(n, px) {
    const pos = new Float32Array(n * 3);
    const col = new Float32Array(n * 3);
    const dirs = new Float32Array(n * 3);
    const spd = new Float32Array(n);
    const palette = [0xff1f8e, 0x19e6e6, 0xffe81a, 0xffffff, 0xff3d9a, 0x19e6e6, 0xffe81a];
    const cc = new THREE.Color();
    for (let i = 0; i < n; i++) {
        const d = new THREE.Vector3(gauss(), gauss() * 0.8, gauss()).normalize();
        dirs.set([d.x, d.y, d.z], i * 3);
        spd[i] = 18 + rand() * 95;
        cc.setHex(palette[(rand() * palette.length) | 0]);
        col.set([cc.r, cc.g, cc.b], i * 3);
    }
    const geo = new THREE.BufferGeometry();
    geo.setAttribute("position", new THREE.BufferAttribute(pos, 3));
    geo.setAttribute("color", new THREE.BufferAttribute(col, 3));
    const mat = new THREE.PointsMaterial({
        size: px, sizeAttenuation: false, vertexColors: true, transparent: true,
        opacity: 1, depthWrite: false, map: dotTex
    });
    const pts = new THREE.Points(geo, mat);
    pts.frustumCulled = false;
    pts.visible = false;
    scene.add(pts);
    confettiGroups.push({ pts, mat, px, n, pos, dirs, spd, geo });
}
const cN = Math.round(260 * quality + 60);
makeConfetti(cN, 5);
makeConfetti(Math.round(cN * 0.7), 9);
makeConfetti(Math.round(cN * 0.35), 15);

/* =====================================================
   ADAPTACIÓN A LA PANTALLA
===================================================== */
let baseR = 34, baseH = 9, textScale = 1;

function fitToScreen() {
    const w = window.innerWidth, h = window.innerHeight, aspect = w / h;
    dpr = Math.min(window.devicePixelRatio || 1, 2);

    camera.aspect = aspect;
    camera.fov = aspect < 1 ? 62 : 55;
    camera.updateProjectionMatrix();

    renderer.setPixelRatio(dpr);
    renderer.setSize(w, h, false);

    baseR = 34 * (aspect < 1 ? 1 + (1 - aspect) * 0.5 : 1);
    baseH = aspect < 1 ? 11 : 9;
    textScale = Math.max(0.6, Math.min(1, aspect * 1.2));

    confettiGroups.forEach(g => { g.mat.size = g.px * dpr; });
}
fitToScreen();

/* =====================================================
   INTERACCIÓN
===================================================== */
let started = false;
let paused = false;
let theta = 0.35;
let height = null;     // desplazamiento de altura por arrastre
let heightOff = 0;
let zoom = 0;

const pointers = new Map();
let pinchStart = 0, zoomStart = 0, dragging = false;

canvas.addEventListener("pointerdown", e => {
    canvas.setPointerCapture && canvas.setPointerCapture(e.pointerId);
    pointers.set(e.pointerId, { x: e.clientX, y: e.clientY });
    dragging = true;
    if (pointers.size === 2) {
        const [a, b] = [...pointers.values()];
        pinchStart = Math.hypot(a.x - b.x, a.y - b.y);
        zoomStart = zoom;
    }
});

canvas.addEventListener("pointermove", e => {
    const p = pointers.get(e.pointerId);
    if (!p) return;
    const dx = e.clientX - p.x, dy = e.clientY - p.y;
    pointers.set(e.pointerId, { x: e.clientX, y: e.clientY });

    if (pointers.size === 2) {
        const [a, b] = [...pointers.values()];
        const d = Math.hypot(a.x - b.x, a.y - b.y);
        if (pinchStart > 0) zoom = Math.max(-16, Math.min(22, zoomStart + (pinchStart - d) * 0.06));
    } else {
        theta -= dx * 0.006;
        heightOff = Math.max(-5, Math.min(18, heightOff + dy * 0.06));
    }
});

function endPointer(e) {
    pointers.delete(e.pointerId);
    pinchStart = 0;
    if (!pointers.size) dragging = false;
}
canvas.addEventListener("pointerup", endPointer);
canvas.addEventListener("pointercancel", endPointer);

canvas.addEventListener("wheel", e => {
    e.preventDefault();
    zoom = Math.max(-16, Math.min(22, zoom + e.deltaY * 0.025));
}, { passive: false });

["gesturestart", "gesturechange", "gestureend"].forEach(n => document.addEventListener(n, e => e.preventDefault()));
document.addEventListener("touchmove", e => { if (!e.target.closest || !e.target.closest(".panel, .lock")) e.preventDefault(); }, { passive: false });

const pauseBtn = document.getElementById("pauseBtn");
const pauseIcon = document.getElementById("pauseIcon");
const PAUSE_SVG = '<rect x="6" y="5" width="4" height="14" rx="1"/><rect x="14" y="5" width="4" height="14" rx="1"/>';
const PLAY_SVG = '<path d="M7 4.5v15l12-7.5z"/>';

document.getElementById("startBtn").addEventListener("click", () => {
    document.getElementById("intro").classList.add("hidden");
    started = true;
    onStartMusic();
    confettiGroups.forEach(g => { g.pts.visible = true; });
    setTimeout(() => { pauseBtn.classList.add("show"); hintEl.classList.add("show"); }, 3500);
    setTimeout(() => hintEl.classList.remove("show"), 12000);
});

pauseBtn.addEventListener("click", () => {
    paused = !paused;
    pauseIcon.innerHTML = paused ? PLAY_SVG : PAUSE_SVG;
});

/* ---------- pantalla completa (donde el navegador lo permita) ---------- */
const fsBtn = document.getElementById("fsBtn");
const rootEl = document.documentElement;
const canFS = !!(rootEl.requestFullscreen || rootEl.webkitRequestFullscreen);
if (canFS) {
    fsBtn.style.display = "flex";
    fsBtn.addEventListener("click", () => {
        try {
            const inFS = document.fullscreenElement || document.webkitFullscreenElement;
            if (inFS) (document.exitFullscreen || document.webkitExitFullscreen).call(document);
            else (rootEl.requestFullscreen || rootEl.webkitRequestFullscreen).call(rootEl);
        } catch (err) {}
    });
}

window.addEventListener("resize", fitToScreen);
window.addEventListener("orientationchange", () => setTimeout(fitToScreen, 250));

let tabHidden = false;
document.addEventListener("visibilitychange", () => { tabHidden = document.hidden; });


/* =====================================================
   ✎ PERSONALIZACIÓN: almacenamiento, música, panel y contraseña
===================================================== */
const $ = id => document.getElementById(id);
const toastEl = $("toast");
let toastT;
function toast(msg) {
    toastEl.textContent = msg;
    toastEl.classList.add("show");
    clearTimeout(toastT);
    toastT = setTimeout(() => toastEl.classList.remove("show"), 2600);
}

/* ---------- almacenamiento (IndexedDB, con respaldo en memoria) ---------- */
const DB = (function () {
    let db = null;
    const mem = {};
    return {
        open() {
            return new Promise(res => {
                try {
                    const r = indexedDB.open("galaxy-gallery", 1);
                    r.onupgradeneeded = () => r.result.createObjectStore("kv");
                    r.onsuccess = () => { db = r.result; res(); };
                    r.onerror = () => res();
                    r.onblocked = () => res();
                } catch (e) { res(); }
            });
        },
        get(k) {
            return new Promise(res => {
                if (!db) return res(mem[k]);
                try {
                    const q = db.transaction("kv").objectStore("kv").get(k);
                    q.onsuccess = () => res(q.result);
                    q.onerror = () => res(mem[k]);
                } catch (e) { res(mem[k]); }
            });
        },
        set(k, v) {
            mem[k] = v;
            return new Promise(res => {
                if (!db) return res();
                try {
                    const tx = db.transaction("kv", "readwrite");
                    tx.objectStore("kv").put(v, k);
                    tx.oncomplete = () => res();
                    tx.onerror = () => res();
                    tx.onabort = () => { toast("No se pudo guardar (poco espacio)"); res(); };
                } catch (e) { res(); }
            });
        },
        del(k) {
            delete mem[k];
            return new Promise(res => {
                if (!db) return res();
                try {
                    const tx = db.transaction("kv", "readwrite");
                    tx.objectStore("kv").delete(k);
                    tx.oncomplete = () => res();
                    tx.onerror = () => res();
                } catch (e) { res(); }
            });
        }
    };
})();

/* ---------- aplicar configuración a la escena ---------- */
function setPhrases(list) {
    textTextures.forEach(tx => tx.dispose());
    textTextures = list.map(p => textTexture(p.toUpperCase()));
    textItems.forEach((it, i) => {
        it.mesh.material.map = textTextures[i % textTextures.length];
        it.mesh.material.needsUpdate = true;
    });
}

function applyAll() {
    $("title").textContent = cfg.title;
    $("subtitle").textContent = cfg.subtitle;
    setPhrases(cfg.phrases);
    buildPhotos();
    const img = $("coverImg"), svg = $("bearsSvg");
    if (cover) { img.src = cover; img.style.display = "block"; svg.style.display = "none"; }
    else { img.style.display = "none"; svg.style.display = ""; }
}

/* ---------- música (Web Audio) ---------- */
let actx = null, gainNode = null, srcNode = null;
let musicBuf = null, musicName = "", musicOn = false, wantMusic = false;
const musicBtn = $("musicBtn");

function getCtx() {
    if (!actx) {
        const AC = window.AudioContext || window.webkitAudioContext;
        if (!AC) return null;
        actx = new AC();
        gainNode = actx.createGain();
        gainNode.gain.value = 0.9;
        gainNode.connect(actx.destination);
    }
    return actx;
}
function decodeAudio(raw) {
    return new Promise((res, rej) => {
        const c = getCtx();
        if (!c) return rej(new Error("sin audio"));
        const p = c.decodeAudioData(raw.slice(0), res, rej);
        if (p && p.catch) p.catch(rej);
    });
}
function updateMusicUI() {
    musicBtn.style.display = (musicBuf && started) ? "flex" : "none";
    musicBtn.classList.toggle("off", !musicOn);
}
function stopMusic() {
    try { srcNode && srcNode.stop(); } catch (e) {}
    srcNode = null; musicOn = false; updateMusicUI();
}
function playMusic() {
    const c = getCtx();
    if (!c || !musicBuf) return;
    if (c.resume) c.resume();
    try { srcNode && srcNode.stop(); } catch (e) {}
    srcNode = c.createBufferSource();
    srcNode.buffer = musicBuf;
    srcNode.loop = true;
    srcNode.connect(gainNode);
    srcNode.start(0);
    musicOn = true; updateMusicUI();
}
function onStartMusic() {
    wantMusic = true;
    const c = getCtx();
    if (c && c.resume) c.resume();   // desbloquea el audio dentro del toque
    if (musicBuf && !musicOn) playMusic();
    updateMusicUI();
}
async function loadMusic(raw, name) {
    stopMusic();
    try { musicBuf = await decodeAudio(raw); musicName = name; }
    catch (e) { musicBuf = null; musicName = ""; toast("No pude leer esa canción"); updateMusicUI(); return false; }
    if (wantMusic) playMusic(); else updateMusicUI();
    return true;
}
function clearMusic() { stopMusic(); musicBuf = null; musicName = ""; updateMusicUI(); }

musicBtn.addEventListener("click", () => {
    if (!musicBuf) return;
    if (musicOn) { try { srcNode && srcNode.stop(); } catch (e) {} srcNode = null; musicOn = false; wantMusic = false; updateMusicUI(); }
    else { wantMusic = true; playMusic(); }
});
document.addEventListener("visibilitychange", () => {
    if (!actx) return;
    if (document.hidden) { actx.suspend && actx.suspend(); }
    else if (musicOn) { actx.resume && actx.resume(); }
});

/* ---------- lectura de archivos ---------- */
function readAs(file, mode) {
    return new Promise((res, rej) => {
        const r = new FileReader();
        r.onload = () => res(r.result);
        r.onerror = () => rej(r.error);
        mode === "buffer" ? r.readAsArrayBuffer(file) : r.readAsDataURL(file);
    });
}
function loadImg(src) {
    return new Promise((res, rej) => { const i = new Image(); i.onload = () => res(i); i.onerror = rej; i.src = src; });
}
async function squareDataURL(file, size) {
    const img = await loadImg(await readAs(file, "url"));
    const m = Math.min(img.width, img.height);
    const c = document.createElement("canvas");
    c.width = c.height = size;
    c.getContext("2d").drawImage(img, (img.width - m) / 2, (img.height - m) / 2, m, m, 0, 0, size, size);
    return c.toDataURL("image/jpeg", 0.86);
}
async function fitDataURL(file, max) {
    const img = await loadImg(await readAs(file, "url"));
    const k = Math.min(1, max / Math.max(img.width, img.height));
    const c = document.createElement("canvas");
    c.width = Math.round(img.width * k); c.height = Math.round(img.height * k);
    c.getContext("2d").drawImage(img, 0, 0, c.width, c.height);
    return c.toDataURL(/png|webp|gif/i.test(file.type) ? "image/png" : "image/jpeg", 0.9);
}

/* ---------- contraseña ---------- */
let lockHash = null;
async function hashPass(p) {
    const str = "galaxy-gallery::" + p;
    try {
        if (window.crypto && crypto.subtle) {
            const buf = await crypto.subtle.digest("SHA-256", new TextEncoder().encode(str));
            return Array.from(new Uint8Array(buf)).map(b => b.toString(16).padStart(2, "0")).join("");
        }
    } catch (e) {}
    let h = 5381;
    for (let r = 0; r < 3; r++) for (let i = 0; i < str.length; i++) h = ((h << 5) + h + str.charCodeAt(i)) | 0;
    return "f" + h;
}
function updateLockStatus() {
    $("lockStatus").textContent = lockHash ? "🔒 Protegido con contraseña" : "🔓 Sin contraseña (cualquiera con el enlace puede editar en su dispositivo)";
}
const lockEl = $("lock"), lockIn = $("lockIn"), lockErr = $("lockErr");
function showLock() { lockIn.value = ""; lockErr.textContent = ""; lockEl.classList.add("open"); setTimeout(() => lockIn.focus(), 80); }
async function tryUnlock() {
    const h = await hashPass(lockIn.value);
    if (h === lockHash) { lockEl.classList.remove("open"); openPanel(); }
    else { lockErr.textContent = "Contraseña incorrecta"; lockIn.value = ""; }
}
$("lockOk").addEventListener("click", tryUnlock);
lockIn.addEventListener("keydown", e => { if (e.key === "Enter") tryUnlock(); });
$("lockCancel").addEventListener("click", () => lockEl.classList.remove("open"));

$("setPass").addEventListener("click", async () => {
    const v = $("inPass").value;
    if (v.length < 4) { toast("Mínimo 4 caracteres"); return; }
    lockHash = await hashPass(v);
    await DB.set("lock", lockHash);
    $("inPass").value = "";
    updateLockStatus();
    toast("Contraseña guardada 🔒");
});
$("rmPass").addEventListener("click", async () => {
    lockHash = null;
    await DB.del("lock");
    updateLockStatus();
    toast("Contraseña quitada");
});

/* ---------- panel ---------- */
let draft = null;
const panel = $("panel");

function openPanel() {
    draft = { photos: photos.slice(), cover: cover, music: undefined };
    $("inTitle").value = cfg.title;
    $("inSub").value = cfg.subtitle;
    $("inPhrases").value = cfg.phrases.join("\n");
    $("inPass").value = "";
    updateLockStatus();
    renderPanel();
    panel.scrollTop = 0;
    panel.classList.add("open");
    if (!lockHash) setTimeout(() => toast("Tip: pon una contraseña al final del panel"), 600);
}
function closePanel() { panel.classList.remove("open"); draft = null; }

function renderPanel() {
    const grid = $("photoGrid");
    grid.innerHTML = "";
    draft.photos.forEach((src, i) => {
        const d = document.createElement("div");
        d.className = "thumb";
        const im = document.createElement("img");
        im.src = src; im.alt = "";
        const x = document.createElement("button");
        x.type = "button"; x.textContent = "✕"; x.setAttribute("aria-label", "Quitar foto");
        x.addEventListener("click", () => { draft.photos.splice(i, 1); renderPanel(); });
        d.appendChild(im); d.appendChild(x);
        grid.appendChild(d);
    });
    const cp = $("coverPrev");
    if (draft.cover) { cp.src = draft.cover; cp.style.display = "block"; } else { cp.style.display = "none"; }
    let label = "Sin música";
    if (draft.music === undefined) { if (musicBuf) label = "🎵 " + musicName; }
    else if (draft.music) label = "🎵 " + draft.music.name;
    $("musicLabel").textContent = label;
}

$("inPhotos").addEventListener("change", async e => {
    const files = [...e.target.files];
    e.target.value = "";
    for (const f of files) {
        if (draft.photos.length >= 12) { toast("Máximo 12 fotos"); break; }
        try { draft.photos.push(await squareDataURL(f, 384)); }
        catch (err) { toast("No pude leer una de las fotos"); }
    }
    renderPanel();
});
$("inCover").addEventListener("change", async e => {
    const f = e.target.files[0];
    e.target.value = "";
    if (!f) return;
    try { draft.cover = await fitDataURL(f, 480); } catch (err) { toast("No pude leer esa imagen"); }
    renderPanel();
});
$("rmCover").addEventListener("click", () => { draft.cover = null; renderPanel(); });
$("inMusic").addEventListener("change", async e => {
    const f = e.target.files[0];
    e.target.value = "";
    if (!f) return;
    if (f.size > 20 * 1024 * 1024) { toast("La canción pesa más de 20 MB"); return; }
    try { draft.music = { raw: await readAs(f, "buffer"), name: f.name }; }
    catch (err) { toast("No pude leer esa canción"); }
    renderPanel();
});
$("rmMusic").addEventListener("click", () => { draft.music = null; renderPanel(); });

$("saveP").addEventListener("click", async () => {
    cfg.title = $("inTitle").value.trim() || DEFAULTS.title;
    cfg.subtitle = $("inSub").value.trim() || DEFAULTS.subtitle;
    const lines = $("inPhrases").value.split("\n").map(l => l.trim().slice(0, 40)).filter(Boolean).slice(0, 8);
    cfg.phrases = lines.length ? lines : DEFAULTS.phrases.slice();
    photos = draft.photos.slice();
    cover = draft.cover;
    applyAll();

    await DB.set("cfg", cfg);
    await DB.set("photos", photos);
    await DB.set("cover", cover);

    if (draft.music !== undefined) {
        if (draft.music) {
            await DB.set("music", { name: draft.music.name, data: draft.music.raw });
            await loadMusic(draft.music.raw, draft.music.name);
        } else {
            await DB.del("music");
            clearMusic();
        }
    }
    closePanel();
    toast("Guardado ✓");
});

let resetArmed = false;
$("resetP").addEventListener("click", async () => {
    const b = $("resetP");
    if (!resetArmed) {
        resetArmed = true; b.textContent = "¿Seguro? Toca otra vez";
        setTimeout(() => { resetArmed = false; b.textContent = "Restaurar"; }, 3000);
        return;
    }
    resetArmed = false; b.textContent = "Restaurar";
    cfg = { title: DEFAULTS.title, subtitle: DEFAULTS.subtitle, phrases: DEFAULTS.phrases.slice() };
    photos = []; cover = null;
    await DB.del("cfg"); await DB.del("photos"); await DB.del("cover"); await DB.del("music");
    clearMusic();
    applyAll();
    closePanel();
    toast("Restaurado al diseño original");
});

$("closeP").addEventListener("click", closePanel);

/* ---------- botón ✎ ---------- */
const ready = (async function () {
    try {
        await DB.open();
        const c = await DB.get("cfg");
        if (c) {
            cfg.title = c.title || DEFAULTS.title;
            cfg.subtitle = c.subtitle || DEFAULTS.subtitle;
            cfg.phrases = (c.phrases && c.phrases.length) ? c.phrases : DEFAULTS.phrases.slice();
        }
        const p = await DB.get("photos");
        if (Array.isArray(p)) photos = p;
        cover = (await DB.get("cover")) || null;
        lockHash = (await DB.get("lock")) || null;
        applyAll();
        const m = await DB.get("music");
        if (m && m.data) await loadMusic(m.data, m.name);
    } catch (e) { applyAll(); }
})();

$("editBtn").addEventListener("click", async () => {
    await ready;
    if (lockHash) showLock(); else openPanel();
});

/* =====================================================
   BUCLE PRINCIPAL
===================================================== */
let st = 0;            // tiempo desde que se pulsa INICIAR
let t = 0;             // tiempo general de animación
let last = performance.now();
const camPos = new THREE.Vector3(0, 70, 130);
const START_R = 130, START_H = 62;
const BURST_END = 5;

function frame(now) {
    requestAnimationFrame(frame);
    if (tabHidden) { last = now; return; }

    let dt = Math.min((now - last) / 1000, 0.05);
    last = now;
    if (paused) dt = 0;

    t += dt;
    if (started) st += dt;

    // --- cámara ---
    const k = started ? ease(st / 8) : 0;
    const R = START_R + (baseR + zoom - START_R) * k;
    const H = START_H + (baseH + heightOff - START_H) * k;
    if (!dragging) theta += 0.035 * dt;
    const target = new THREE.Vector3(Math.sin(theta) * R, H, Math.cos(theta) * R);
    camPos.lerp(target, dragging ? 0.35 : 0.12);
    camera.position.copy(camPos);
    camera.lookAt(0, 4.5 * k, 0);

    // --- galaxia ---
    galaxyPts.rotation.y += 0.03 * dt;
    diskGlow.material.opacity = 0.7 + Math.sin(t * 1.6) * 0.12;

    // --- corazón ---
    const heartReveal = smooth((st - 1.8) / 2.5);
    hMat.opacity = heartReveal * (0.82 + Math.sin(t * 2.1) * 0.16);
    tipGlow.material.opacity = heartReveal * (0.75 + Math.sin(t * 3) * 0.2);
    notchGlow.material.opacity = heartReveal * 0.65;
    heartGroup.position.y = 1.4 + HEART_S * 1.06 + Math.sin(t * 1.2) * 0.15;
    heartGroup.rotation.y = Math.atan2(camera.position.x, camera.position.z);

    // --- textos, fotos y corazones ---
    const ui = smooth((st - 4.2) / 3);

    textItems.forEach((it, i) => {
        it.angle += it.speed * dt;
        const m = it.mesh;
        m.position.set(Math.cos(it.angle) * it.radius, it.y + Math.sin(t * 0.8 + it.phase) * 0.25, Math.sin(it.angle) * it.radius);
        m.rotation.y = Math.atan2(camera.position.x - m.position.x, camera.position.z - m.position.z);
        m.scale.set(textScale, textScale, 1);
        m.material.opacity = ui * (0.7 + 0.3 * Math.sin(t * 1.8 + i));
    });

    photoItems.forEach(it => {
        it.angle += it.speed * dt;
        it.sprite.position.set(Math.cos(it.angle) * it.radius, it.y + Math.sin(t * 0.9 + it.phase) * 0.5, Math.sin(it.angle) * it.radius);
        it.sprite.material.opacity = ui;
    });

    floatHearts.forEach(it => {
        it.angle += it.speed * dt;
        const pulse = 1 + Math.sin(t * 2.4 + it.phase) * 0.08;
        it.sprite.position.set(Math.cos(it.angle) * it.radius, it.y + Math.sin(t * 0.7 + it.phase) * 1.1, Math.sin(it.angle) * it.radius);
        it.sprite.scale.set(it.size * pulse, it.size * pulse, 1);
        it.sprite.material.opacity = ui * 0.9;
    });

    // --- confeti ---
    if (started) {
        confettiGroups.forEach(g => {
            if (!g.pts.visible) return;
            if (st > BURST_END) { g.pts.visible = false; return; }
            const dist = (1 - Math.exp(-0.55 * st)) / 0.55;
            for (let i = 0; i < g.n; i++) {
                const d = dist * g.spd[i];
                g.pos[i * 3]     = g.dirs[i * 3] * d;
                g.pos[i * 3 + 1] = g.dirs[i * 3 + 1] * d;
                g.pos[i * 3 + 2] = g.dirs[i * 3 + 2] * d;
            }
            g.geo.attributes.position.needsUpdate = true;
            g.mat.opacity = 1 - smooth((st - 2.6) / 2.3);
        });
    }

    renderer.render(scene, camera);
}
requestAnimationFrame(frame);

})();
</script>

</body>
</html>