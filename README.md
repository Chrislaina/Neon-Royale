<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>Neon Royale — Game Modes</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Exo+2:wght@300;400;600;700&display=swap" rel="stylesheet">
  <style>
    :root{
      --acc: #00e5ff;
      --acc-soft: rgba(0,229,255,0.35);
      --acc-strong: rgba(0,229,255,0.8);
      --bg1: #070817;
      --bg2: #0a1230;
      --text: #e6f7ff;
      --goal: #22d3ee;
    }
    html, body { height: 100%; }
    body{
      font-family: 'Exo 2', system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial;
      color: var(--text);
      background: radial-gradient(1200px 800px at 70% -20%, rgba(0,229,255,0.08), transparent 60%),
                  radial-gradient(900px 700px at 20% -10%, rgba(157,78,221,0.08), transparent 60%),
                  linear-gradient(160deg, var(--bg1), var(--bg2));
      overflow-x: hidden;
    }
    .title-font{ font-family: 'Orbitron', sans-serif; letter-spacing: 0.04em; }
    .neon-border{
      position: relative;
      box-shadow:
        0 0 0 1px var(--acc) inset,
        0 0 18px var(--acc-soft),
        0 0 46px rgba(0,229,255,0.18);
      border-radius: 14px;
      background: rgba(255,255,255,0.02);
      backdrop-filter: blur(6px);
    }
    .neon-btn{
      position: relative;
      border-radius: 12px;
      background: linear-gradient(180deg, rgba(0,229,255,0.12), rgba(0,229,255,0.04));
      box-shadow:
        0 0 0 1px var(--acc) inset,
        0 0 12px var(--acc-soft),
        inset 0 6px 24px rgba(255,255,255,0.05);
      color: var(--text);
      transition: transform .18s ease, box-shadow .18s ease, filter .18s ease, opacity .18s ease;
    }
    .neon-btn:hover{ transform: translateY(-1px); box-shadow: 0 0 0 1px var(--acc) inset, 0 0 30px var(--acc-strong), 0 0 60px rgba(0,229,255,0.35); filter: drop-shadow(0 0 8px var(--acc-soft)); }
    .tab-btn{ border-radius: 12px; padding: 10px 14px; }
    .tab-btn.active{
      background: linear-gradient(180deg, rgba(0,229,255,0.22), rgba(0,229,255,0.06));
      box-shadow: 0 0 0 1px var(--acc) inset, 0 0 20px var(--acc-soft), inset 0 8px 28px rgba(255,255,255,0.06);
    }

    .gridlines::before{
      content:""; position:absolute; inset:0;
      background:
        linear-gradient(transparent 23px, rgba(0,229,255,0.065) 24px) 0 0 / 100% 24px,
        linear-gradient(90deg, transparent 23px, rgba(0,229,255,0.065) 24px) 0 0 / 24px 100%;
      mask-image: radial-gradient(circle at center, rgba(255,255,255,0.65), transparent 60%);
      opacity:.6; pointer-events:none; animation: gridmove 18s linear infinite;
    }
    @keyframes gridmove{ 0% { transform: translateY(0) } 100% { transform: translateY(24px) } }

    /* Arena */
    .track{
      position: relative;
      height: 70vh;
      min-height: 360px;
      border-radius: 14px;
      overflow:hidden;
      background: linear-gradient(180deg, rgba(255,255,255,0.03), rgba(255,255,255,0.02));
      box-shadow: 0 0 0 1px rgba(255,255,255,0.08) inset, 0 0 22px rgba(0,229,255,0.2);
    }
    .maze-wall{
      position:absolute;
      background: linear-gradient(180deg, rgba(0,229,255,0.18), rgba(168,85,247,0.28));
      box-shadow: 0 0 0 1px rgba(255,255,255,0.08) inset, 0 0 12px rgba(0,229,255,0.25);
    }
    .player{
      position:absolute; width: 34px; height: 34px; border-radius: 10px; display:grid; place-items:center;
      box-shadow: 0 0 0 1px var(--acc) inset, 0 0 28px var(--acc-soft);
      transition: transform .12s ease;
      z-index: 20;
      background: linear-gradient(180deg, rgba(255,255,255,0.06), rgba(255,255,255,0.02));
    }
    .exit{
      position:absolute; width: 36px; height: 36px; border-radius: 10px;
      box-shadow:
        0 0 0 1px var(--goal) inset,
        0 0 18px rgba(34,211,238,0.6),
        inset 0 10px 24px rgba(255,255,255,0.12);
      background: linear-gradient(180deg, rgba(34,211,238,0.25), rgba(34,211,238,0.12));
      z-index: 4;
    }
    .hud-tag{ position:absolute; top:10px; left:12px; font-size: 12px; color: #cffafe; opacity:.85; text-shadow: 0 0 8px rgba(0,0,0,0.6); z-index: 6; }

    .confetti{ position:absolute; inset:0; pointer-events:none; }
    .confetti span{ position:absolute; top:-10px; width:6px; height:10px; border-radius:2px; animation: drop 1.4s linear forwards; }
    @keyframes drop{ to{ transform: translateY(120%) rotate(160deg); opacity: 0.4 } }

    /* Loading overlay */
    .loading-overlay{
      position:fixed; inset:0; z-index:50;
      background: radial-gradient(700px 500px at 50% -10%, rgba(0,229,255,0.08), transparent 60%), #050613;
      display:flex; align-items:center; justify-content:center; flex-direction:column; gap:22px;
    }
    .logo-stroke{
      stroke: var(--acc); stroke-width: 2.5; fill: transparent;
      stroke-dasharray: 680; stroke-dashoffset: 680;
      animation: drawlogo 1.3s ease forwards .3s;
    }
    @keyframes drawlogo{ to{ stroke-dashoffset: 0 } }

    /* Mode select hero card */
    .mode-hero{
      min-height: 60vh;
      background:
        radial-gradient(600px 260px at 50% 0%, rgba(0,229,255,0.18), transparent 65%),
        linear-gradient(180deg, rgba(255,255,255,0.03), rgba(255,255,255,0.015));
    }

    /* Crate modal */
    .modal{
      position: fixed; inset:0; display:none; align-items:center; justify-content:center; z-index:60;
      background: rgba(5,6,19,0.6); backdrop-filter: blur(6px);
    }
    .crate-box{
      width: 360px; height: 260px; border-radius: 16px; position:relative; overflow:hidden;
      background: radial-gradient(400px 200px at 50% 0%, rgba(0,229,255,0.2), transparent 60%), rgba(255,255,255,0.03);
      box-shadow: 0 0 0 1px rgba(255,255,255,0.09) inset, 0 0 36px rgba(0,229,255,0.25);
    }
    .crate{ position:absolute; inset:0; display:flex; align-items:center; justify-content:center; flex-direction:column; gap:12px; }
    .crate svg{ filter: drop-shadow(0 0 14px rgba(0,229,255,0.4)); }
    .shake{ animation: shake 1.1s ease-in-out infinite; }
    @keyframes shake{
      0%,100%{ transform: translateY(0) rotate(0) }
      20%{ transform: translateY(-2px) rotate(-1deg) }
      40%{ transform: translateY(2px) rotate(1deg) }
      60%{ transform: translateY(-1px) rotate(-1deg) }
      80%{ transform: translateY(1px) rotate(1deg) }
    }
    .beam{ position:absolute; top:0; left:50%; transform:translateX(-50%); width: 160px; height: 0; background: radial-gradient(80px 40px at 50% 0, rgba(255,255,255,0.8), rgba(34,211,238,0.3), transparent 70%); animation: beam 1.1s ease forwards; filter: blur(1px); }
    @keyframes beam{ to{ height: 260px; } }
    .reveal{ animation: pop .35s ease forwards; transform: scale(0.7); opacity:0; }
    @keyframes pop{ to{ transform: scale(1); opacity:1 } }

    .reduced *{ box-shadow: none !important; filter:none !important; text-shadow:none !important; }
  </style>
</head>
<body>
  <!-- Loading Overlay (only screen without header) -->
  <div id="loadingOverlay" class="loading-overlay">
    <svg width="560" height="80" viewBox="0 0 560 80" aria-label="Neon Royale">
      <text x="50%" y="50%" dominant-baseline="middle" text-anchor="middle" class="logo-stroke title-font" font-size="44">NEON ROYALE</text>
    </svg>
    <div class="w-64 h-2 rounded-full overflow-hidden neon-border">
      <div id="loadBar" class="h-full" style="width:0%; background: var(--acc)"></div>
    </div>
    <p class="text-[13px] text-cyan-200/80">Booting systems • Preparing modes • Charging glow</p>
  </div>

  <!-- App Shell -->
  <div id="app" class="min-h-screen opacity-0 transition-opacity duration-500">
    <!-- Global Header (always visible except during loading) -->
    <header class="sticky top-0 z-40 backdrop-blur-md bg-black/30">
      <div class="max-w-7xl mx-auto px-5 py-4 flex items-center gap-4">
        <div class="flex items-center gap-3">
          <div class="title-font text-xl">NEON ROYALE</div>
          <div class="text-[11px] text-cyan-200/70 -mt-0.5">Arcade</div>
        </div>
        <nav class="ml-auto hidden md:flex items-center gap-2">
          <button data-tab="play" class="tab-btn neon-btn">Play</button>
          <button data-tab="shop" class="tab-btn neon-btn">Shop</button>
          <button data-tab="settings" class="tab-btn neon-btn">Settings</button>
          <button data-tab="customize" class="tab-btn neon-btn">Customize</button>
          <div class="ml-4 flex items-center gap-2 text-cyan-100/90">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="9" stroke="currentColor"/><path d="M8 12h8M12 8v8" stroke="currentColor"/></svg>
            <span id="coinCount" class="title-font">0</span>
          </div>
        </nav>
        <div class="md:hidden ml-auto">
          <div class="flex items-center gap-2 mb-2 text-cyan-100/90">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="9" stroke="currentColor"/><path d="M8 12h8M12 8v8" stroke="currentColor"/></svg>
            <span id="coinCountM" class="title-font">0</span>
          </div>
          <select id="mobileTabs" class="neon-border bg-transparent px-3 py-2 rounded-lg">
            <option value="play">Play</option>
            <option value="shop">Shop</option>
            <option value="settings">Settings</option>
            <option value="customize">Customize</option>
          </select>
        </div>
      </div>
      <div class="h-px bg-gradient-to-r from-transparent via-cyan-400/30 to-transparent"></div>
    </header>

    <!-- MODE SELECT SCREEN -->
    <main id="screen-mode" class="max-w-7xl mx-auto px-5 py-10">
      <div class="neon-border mode-hero grid place-items-center p-10">
        <div class="text-center max-w-2xl">
          <h1 class="title-font text-3xl md:text-4xl">Choose a Mode</h1>
          <p class="text-cyan-100/80 mt-2">More modes coming soon. Start with Maze Mode!</p>
          <div class="mt-6">
            <button id="enterMaze" class="neon-btn px-6 py-3 text-lg">Maze Mode</button>
          </div>
        </div>
      </div>
    </main>

    <!-- MAZE APP SCREEN (hidden until Maze Mode is entered) -->
    <main id="screen-maze" class="hidden max-w-7xl mx-auto px-5 py-8 relative gridlines">
      <!-- PLAY -->
      <section id="tab-play">
        <div class="neon-border p-5 md:p-7">
          <div class="flex flex-col md:flex-row md:items-center gap-4 justify-between">
            <div>
              <h2 class="title-font text-2xl md:text-3xl">Neon Maze</h2>
              <p class="text-cyan-100/80 text-sm md:text-base mt-1">Full-screen maze. Find the glowing exit.</p>
              <p class="text-xs text-cyan-200/70 mt-1">Controls: WASD or Arrow Keys to move.</p>
            </div>
            <div class="flex items-center gap-2">
              <button id="startBtn" class="neon-btn px-4 py-2.5">Start Round</button>
              <button id="regenBtn" class="neon-btn px-4 py-2.5 opacity-80">New Maze</button>
            </div>
          </div>

          <!-- Arena -->
          <div class="mt-6 track neon-border relative" id="arena">
            <div class="hud-tag" id="hudLabel">Level: Maze • Reach the glowing exit</div>
            <div id="confetti" class="confetti"></div>
          </div>

          <div class="mt-4 flex items-center gap-3">
            <div id="timer" class="text-sm text-cyan-200/90">00:00</div>
            <div id="status" class="text-sm text-cyan-100/85"></div>
            <div class="ml-auto flex items-center gap-4">
              <div class="flex items-center gap-2 text-cyan-100/90">
                <svg width="18" height="18" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="9" stroke="currentColor"/><path d="M8 12h8M12 8v8" stroke="currentColor"/></svg>
                <span id="coinCountPlay" class="title-font">0</span>
              </div>
              <div class="flex items-center gap-2">
                <span class="text-xs text-cyan-200/70">You</span>
                <div id="miniAvatar" class="neon-border rounded-full p-1"></div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- SHOP -->
      <section id="tab-shop" class="hidden">
        <div class="neon-border p-5 md:p-7 space-y-6">
          <div class="flex items-center justify-between">
            <div>
              <h2 class="title-font text-2xl md:text-3xl">Shop</h2>
              <p class="text-cyan-100/80 text-sm mt-1">Buy crates, unlock cosmetics, and equip them.</p>
            </div>
            <div class="flex items-center gap-2 text-cyan-100/90">
              <svg width="18" height="18" viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="9" stroke="currentColor"/><path d="M8 12h8M12 8v8" stroke="currentColor"/></svg>
              <span id="coinCountShop" class="title-font">0</span>
            </div>
          </div>

          <div class="grid md:grid-cols-3 gap-5">
            <div class="neon-border p-4">
              <h3 class="title-font text-lg">Mini Crate</h3>
              <p class="text-cyan-200/80 text-sm mt-1">Cost: 1 Coin • Patterns and accessories (no colors)</p>
              <button class="neon-btn mt-3 px-4 py-2.5" data-crate="mini">Buy Mini</button>
            </div>
            <div class="neon-border p-4">
              <h3 class="title-font text-lg">Standard Crate</h3>
              <p class="text-cyan-200/80 text-sm mt-1">Cost: 3 Coins • Mix of items</p>
              <button class="neon-btn mt-3 px-4 py-2.5" data-crate="standard">Buy Standard</button>
            </div>
            <div class="neon-border p-4">
              <h3 class="title-font text-lg">Elite Crate</h3>
              <p class="text-cyan-200/80 text-sm mt-1">Cost: 7 Coins • Higher chance of rare accessories</p>
              <button class="neon-btn mt-3 px-4 py-2.5" data-crate="elite">Buy Elite</button>
            </div>
          </div>

          <div class="neon-border p-5">
            <h3 class="title-font text-xl">Your Inventory</h3>
            <ul id="inventory" class="mt-3 space-y-2 text-sm text-cyan-100/85"></ul>
          </div>
        </div>
      </section>

      <!-- SETTINGS -->
      <section id="tab-settings" class="hidden">
        <div class="neon-border p-5 md:p-7 space-y-5">
          <h2 class="title-font text-2xl md:text-3xl">Settings</h2>
          <div class="grid md:grid-cols-2 gap-5">
            <div class="neon-border p-4">
              <div class="flex items-center justify-between">
                <div>
                  <div class="text-sm text-cyan-200/80">Visual Effects</div>
                  <div class="text-xs text-cyan-300/70">Reduce glow and animations</div>
                </div>
                <label class="inline-flex items-center cursor-pointer">
                  <input id="fxToggle" type="checkbox" class="sr-only peer">
                  <div class="w-11 h-6 bg-cyan-400/20 peer-checked:bg-cyan-400/40 rounded-full relative after:content-[''] after:absolute after:w-5 after:h-5 after:bg-white after:rounded-full after:top-0.5 after:left-0.5 after:transition-all peer-checked:after:translate-x-5"></div>
                </label>
              </div>
            </div>
            <div class="neon-border p-4">
              <div class="text-sm text-cyan-200/80 mb-2">Theme Accent</div>
              <div class="flex items-center gap-2">
                <button data-theme="#00e5ff" class="neon-btn px-3 py-2.5" style="--acc:#00e5ff">Blue</button>
                <button data-theme="#a855f7" class="neon-btn px-3 py-2.5" style="--acc:#a855f7">Purple</button>
                <button data-theme="#22d3ee" class="neon-btn px-3 py-2.5" style="--acc:#22d3ee">Cyan</button>
                <button data-theme="#f472b6" class="neon-btn px-3 py-2.5" style="--acc:#f472b6">Magenta</button>
              </div>
            </div>
            <div class="neon-border p-4">
              <div class="text-sm text-cyan-200/80">Maze Size</div>
              <div class="text-xs text-cyan-300/70">More rows/cols make it harder</div>
              <input id="sizeSlider" type="range" min="1" max="3" value="2" class="w-full mt-2">
              <div class="flex justify-between text-xs text-cyan-200/70 mt-1"><span>Small</span><span>Medium</span><span>Large</span></div>
            </div>
            <div class="neon-border p-4">
              <div class="text-sm text-cyan-200/80">Difficulty</div>
              <div class="text-xs text-cyan-300/70">Controls density and move speed</div>
              <div class="mt-2 grid grid-cols-3 gap-2">
                <button id="dEasy" class="neon-btn px-3 py-2.5">Easy</button>
                <button id="dNormal" class="neon-btn px-3 py-2.5">Normal</button>
                <button id="dHard" class="neon-btn px-3 py-2.5">Hard</button>
              </div>
            </div>
            <div class="neon-border p-4">
              <div class="flex items-center justify-between">
                <div>
                  <div class="text-sm text-cyan-200/80">Music (Demo)</div>
                  <div class="text-xs text-cyan-300/70">Audio not supported here</div>
                </div>
                <label class="inline-flex items-center cursor-not-allowed opacity-60">
                  <input type="checkbox" disabled class="sr-only">
                  <div class="w-11 h-6 bg-cyan-400/20 rounded-full relative after:content-[''] after:absolute after:w-5 after:h-5 after:bg-white after:rounded-full after:top-0.5 after:left-0.5"></div>
                </label>
              </div>
              <input type="range" min="0" max="100" value="40" class="w-full mt-3 opacity-60 cursor-not-allowed">
              <p class="text-[11px] text-cyan-300/70 mt-1">Demo controls only — sound is disabled in this environment.</p>
            </div>
            <div class="neon-border p-4">
              <div class="text-sm text-cyan-200/80">Regenerate</div>
              <div class="text-xs text-cyan-300/70 mb-2">Rebuild the maze with current settings</div>
              <button id="rebuildBtn" class="neon-btn px-3 py-2.5">Rebuild Maze</button>
            </div>
          </div>
        </div>
      </section>

      <!-- CUSTOMIZE -->
      <section id="tab-customize" class="hidden">
        <div class="neon-border p-5 md:p-7">
          <h2 class="title-font text-2xl md:text-3xl">Customize Character</h2>
          <div class="grid md:grid-cols-2 gap-6 mt-5">
            <div class="neon-border p-5 flex items-center justify-center">
              <div id="bigAvatar" class="w-[220px] h-[220px] rounded-[16px] grid place-items-center neon-border"></div>
            </div>
            <div class="neon-border p-5 space-y-4">
              <div>
                <label class="text-sm text-cyan-200/80">Display Name</label>
                <input id="playerName" type="text" maxlength="16" placeholder="Your name"
                  class="mt-1 w-full bg-transparent neon-border p-2 rounded-md" />
              </div>
              <div>
                <label class="text-sm text-cyan-200/80">Primary Color</label>
                <input id="colorPick" type="color" value="#00e5ff" class="mt-1 w-28 h-10 rounded-md">
              </div>
              <div>
                <label class="text-sm text-cyan-200/80">Pattern</label>
                <select id="patternPick" class="mt-1 w-full bg-transparent neon-border p-2 rounded-md">
                  <option value="none">None</option>
                  <option value="stripes">Stripes</option>
                  <option value="dots">Dots</option>
                  <option value="hex">Hex</option>
                </select>
              </div>
              <div>
                <label class="text-sm text-cyan-200/80">Accessory</label>
                <select id="accessoryPick" class="mt-1 w-full bg-transparent neon-border p-2 rounded-md">
                  <option value="none">None</option>
                  <option value="antenna">Antenna</option>
                  <option value="visor">Visor</option>
                  <option value="crown">Crown</option>
                  <option value="halo">Halo</option>
                  <option value="wings">Wings</option>
                </select>
              </div>
              <div class="flex gap-2">
                <button id="saveAvatar" class="neon-btn px-4 py-2.5">Save</button>
                <button id="randomAvatar" class="neon-btn px-4 py-2.5">Randomize</button>
              </div>
              <p class="text-xs text-cyan-300/75">Saved locally. Used in Play view instantly.</p>
            </div>
          </div>
        </div>
      </section>
    </main>
  </div>

  <!-- Crate Animation Modal -->
  <div id="crateModal" class="modal">
    <div class="crate-box neon-border">
      <div id="crateStage" class="crate"></div>
      <div id="crateBeam" class="beam" style="display:none"></div>
    </div>
  </div>

  <script>
    // Helpers
    const $ = (s, r=document)=>r.querySelector(s);
    const $$ = (s, r=document)=>Array.from(r.querySelectorAll(s));
    const sample = a => a[Math.floor(Math.random()*a.length)];

    // Global safety: if anything errors, never get stuck on loader
    (() => {
      function showModesFallback(){
        try{
          const bar = $('#loadBar'), overlay = $('#loadingOverlay'), app = $('#app'), mode = $('#screen-mode');
          if (bar) bar.style.width = '100%';
          if (overlay) overlay.style.display = 'none';
          if (app) app.style.opacity = 1;
          if (mode) mode.classList.remove('hidden');
        }catch(e){}
      }
      window.addEventListener('error', showModesFallback);
      window.addEventListener('unhandledrejection', showModesFallback);
    })();

    // State
    const state = {
      running: false,
      finished: false,
      timerId: null,
      startTime: 0,
      maze: null,
      cols: 19,
      rows: 11,
      cellW: 0,
      cellH: 0,
      margin: 14,
      size: 2,
      difficulty: 'normal',
      theme: '#00e5ff',
      coins: 0,
      inventory: [],
      player: { name:'You', color:'#00e5ff', pattern:'none', accessory:'none', c:0, r:0, el:null },
      exit: { c:0, r:0, el:null },
      lastMoveAt: 0,
      moveDelay: 90
    };

    // Boot
    document.addEventListener('DOMContentLoaded', ()=>{
      try{
        bootLoading();
        // Load saved
        try{
          const wallet = JSON.parse(localStorage.getItem('nr-wallet')||'{}');
          if (wallet.coins !== undefined) state.coins = wallet.coins;
          if (wallet.inventory) state.inventory = wallet.inventory;
          const savedPlayer = JSON.parse(localStorage.getItem('nr-player')||'null');
          if (savedPlayer) Object.assign(state.player, savedPlayer);
        }catch{}
        setupTabs();
        initSettings();
        initCustomize();
        initShop();
        bindMazePlay();
        applyTheme(state.player.color || state.theme);
        renderCoins();
        renderInventory();
        renderMiniAvatar();
        bindModeSelect();
      }catch(e){
        // Absolute safety: even if an init step fails, land on modes
        const bar = $('#loadBar'), overlay = $('#loadingOverlay');
        if (bar) bar.style.width = '100%';
        if (overlay) overlay.style.display = 'none';
        $('#app').style.opacity = 1;
        $('#screen-mode').classList.remove('hidden');
      }
    });

    // Rock-solid loader: timed progress + hard safety
    function bootLoading(){
      const bar = $('#loadBar');
      const overlay = $('#loadingOverlay');
      const app = $('#app');
      const modeScreen = $('#screen-mode');
      let p = 0, shown = false;

      function show(){
        if (shown) return; shown = true;
        if (bar) bar.style.width = '100%';
        setTimeout(()=>{
          if (overlay) overlay.style.display = 'none';
          if (app) app.style.opacity = 1;
          if (modeScreen) modeScreen.classList.remove('hidden');
        }, 150);
      }

      // Smooth fill (setInterval avoids rAF throttling issues)
      const iv = setInterval(()=>{
        p += (100 - p) * 0.22 + 3; // ease towards 100
        if (p >= 100) p = 100;
        if (bar) bar.style.width = Math.round(p) + '%';
        if (p >= 100){
          clearInterval(iv);
          show();
        }
      }, 160);

      // Hard stop after 4s regardless
      setTimeout(()=>{ if (!shown) { clearInterval(iv); p = 100; show(); } }, 4000);
    }

    // Mode select
    function bindModeSelect(){
      const enterBtn = $('#enterMaze');
      if (enterBtn) enterBtn.addEventListener('click', enterMazeMode);
      // If header tabs are used before entering, jump into Maze then switch
      $$('nav [data-tab]').forEach(b=>{
        b.addEventListener('click', ()=>{
          if (!isMazeVisible()) enterMazeMode();
        });
      });
      const mobile = $('#mobileTabs');
      if (mobile){
        mobile.addEventListener('change', ()=>{
          if (!isMazeVisible()) enterMazeMode();
        });
      }
    }
    function isMazeVisible(){ return !$('#screen-maze').classList.contains('hidden'); }
    function enterMazeMode(){
      $('#screen-mode').classList.add('hidden');
      $('#screen-maze').classList.remove('hidden');
      // Switch to Play tab and prep maze
      if (window.switchTab) window.switchTab('play');
      buildMaze();
      renderBigAvatar();
      renderMiniAvatar();
    }

    // Header tabs
    function setupTabs(){
      const buttons = $$('nav [data-tab]');
      const mobile = $('#mobileTabs');
      const activate = (key)=>{
        ['play','shop','settings','customize'].forEach(v => {
          const el = document.getElementById(`tab-${v}`);
          if (el) el.classList.toggle('hidden', v!==key);
        });
        buttons.forEach(b => b.classList.toggle('active', b.dataset.tab===key));
        if (mobile) mobile.value = key;
        if (key==='customize') renderBigAvatar();
        if (key==='shop') renderCoins();
      };
      buttons.forEach(b => b.addEventListener('click', ()=>{
        if (!isMazeVisible()) enterMazeMode();
        activate(b.dataset.tab);
      }));
      if (mobile) mobile.addEventListener('change', e => {
        if (!isMazeVisible()) enterMazeMode();
        activate(e.target.value);
      });
      window.switchTab = activate;
    }

    // Theme
    function applyTheme(color){
      state.theme = color;
      document.documentElement.style.setProperty('--acc', color);
      document.documentElement.style.setProperty('--acc-soft', hexToRgba(color, 0.35));
      document.documentElement.style.setProperty('--acc-strong', hexToRgba(color, 0.8));
      renderMiniAvatar();
      renderBigAvatar();
      if (state.player.el) state.player.el.style.boxShadow = `0 0 0 1px ${color} inset, 0 0 28px ${hexToRgba(color,0.35)}`;
    }
    function hexToRgba(hex, a){
      const m = hex.replace('#','');
      const bigint = parseInt(m.length===3 ? m.split('').map(c=>c+c).join('') : m, 16);
      const r = (bigint >> 16) & 255, g = (bigint >> 8) & 255, b = bigint & 255;
      return `rgba(${r}, ${g}, ${b}, ${a})`;
    }

    // Settings
    function initSettings(){
      const fx = $('#fxToggle');
      if (fx) fx.addEventListener('change', e => {
        document.body.classList.toggle('reduced', e.target.checked);
      });
      $$('#tab-settings [data-theme]').forEach(b => b.addEventListener('click', ()=> applyTheme(b.dataset.theme)));
      const size = $('#sizeSlider'); if (size) size.addEventListener('input', e => { state.size = +e.target.value; });
      const rebuild = $('#rebuildBtn'); if (rebuild) rebuild.addEventListener('click', ()=> { stopGame(); buildMaze(); });
      const setDiff = (d)=>{
        state.difficulty = d;
        ['dEasy','dNormal','dHard'].forEach(id=> { const el=document.getElementById(id); if(el) el.style.opacity = 0.6; });
        if (d==='easy') document.getElementById('dEasy')?.style && (document.getElementById('dEasy').style.opacity = 1);
        if (d==='normal') document.getElementById('dNormal')?.style && (document.getElementById('dNormal').style.opacity = 1);
        if (d==='hard') document.getElementById('dHard')?.style && (document.getElementById('dHard').style.opacity = 1);
        stopGame(); buildMaze();
      };
      document.getElementById('dEasy')?.addEventListener('click', ()=> setDiff('easy'));
      document.getElementById('dNormal')?.addEventListener('click', ()=> setDiff('normal'));
      document.getElementById('dHard')?.addEventListener('click', ()=> setDiff('hard'));
      setTimeout(()=> { const n=document.getElementById('dNormal'); if(n) n.style.opacity = 1; }, 0);
    }

    // Customize
    function initCustomize(){
      const n=$('#playerName'), c=$('#colorPick'), p=$('#patternPick'), a=$('#accessoryPick');
      if (n) n.value = state.player.name;
      if (c) c.value = state.player.color;
      if (p) p.value = state.player.pattern;
      if (a) a.value = state.player.accessory;

      ['input','change'].forEach(evt=>{
        n?.addEventListener(evt, updateAvatarState);
        c?.addEventListener(evt, updateAvatarState);
        p?.addEventListener(evt, updateAvatarState);
        a?.addEventListener(evt, updateAvatarState);
      });

      $('#saveAvatar')?.addEventListener('click', ()=>{
        localStorage.setItem('nr-player', JSON.stringify(state.player));
        toast('Saved!');
      });

      $('#randomAvatar')?.addEventListener('click', ()=>{
        const names = ['Nova','Glitch','Photon','Pixel','Echo','Drift','Quark','Nyx','Orion','Lumen','Cipher','Jinx','Kilo','Omega','Aero'];
        const colors = ['#00e5ff','#22d3ee','#38bdf8','#a855f7','#f472b6','#f59e0b','#10b981','#60a5fa','#06b6d4','#f43f5e'];
        if (n) n.value = sample(names);
        if (c) c.value = sample(colors);
        if (p) p.value = sample(['none','stripes','dots','hex']);
        if (a) a.value = sample(['none','antenna','visor','crown','halo','wings']);
        updateAvatarState();
      });

      renderBigAvatar();
    }
    function updateAvatarState(){
      const n=$('#playerName'), c=$('#colorPick'), p=$('#patternPick'), a=$('#accessoryPick');
      state.player.name = (n?.value || 'You').trim() || 'You';
      state.player.color = c?.value || '#00e5ff';
      state.player.pattern = p?.value || 'none';
      state.player.accessory = a?.value || 'none';
      localStorage.setItem('nr-player', JSON.stringify(state.player));
      renderBigAvatar();
      renderMiniAvatar();
      if (state.player.el){
        state.player.el.innerHTML = avatarSvg(state.player.color, state.player.pattern, 28, state.player.accessory);
      }
    }
    function renderMiniAvatar(){ const box=$('#miniAvatar'); if (box) box.innerHTML = avatarSvg(state.player.color, state.player.pattern, 24, state.player.accessory); }
    function renderBigAvatar(){ const box=$('#bigAvatar'); if (box) box.innerHTML = avatarSvg(state.player.color, state.player.pattern, 160, state.player.accessory); }

    // Shop + Crates
    const crateConfig = {
      // Mini: NO COLORS (only patterns/accessories)
      mini:    { cost:1, pool:{ pattern:0.70, accessory:0.30 }, rare:0.05 },
      standard:{ cost:3, pool:{ color:0.35, pattern:0.35, accessory:0.30 }, rare:0.08 },
      elite:   { cost:7, pool:{ color:0.20, pattern:0.25, accessory:0.55 }, rare:0.25 }
    };
    const colorsPool = ['#00e5ff','#22d3ee','#38bdf8','#a855f7','#f472b6','#f59e0b','#10b981','#60a5fa','#06b6d4','#f43f5e'];
    const patternsPool = ['stripes','dots','hex'];
    const commonAccessories = ['antenna','visor','crown'];
    const rareAccessories = ['halo','wings'];

    function initShop(){
      $$('[data-crate]').forEach(btn=>{
        btn.addEventListener('click', ()=>{
          const key = btn.getAttribute('data-crate');
          openCrate(key);
        });
      });
      renderInventory();
    }

    function pickType(weights){
      const entries = Object.entries(weights);
      const total = entries.reduce((s,[,w])=>s+w,0);
      let r = Math.random()*total;
      for (const [k,w] of entries){
        if ((r -= w) <= 0) return k;
      }
      return entries[0]?.[0] || 'pattern';
    }

    function openCrate(key){
      const cfg = crateConfig[key];
      if (!cfg) return;
      if (!spendCoins(cfg.cost)){
        toast('Not enough coins!');
        renderCoins();
        return;
      }
      renderCoins();
      // Animation UI
      const modal = $('#crateModal');
      const stage = $('#crateStage');
      const beam = $('#crateBeam');
      if (!modal || !stage || !beam) return;

      stage.innerHTML = `
        <div class="shake">
          <svg width="120" height="120" viewBox="0 0 120 120" aria-label="Crate">
            <rect x="20" y="30" width="80" height="70" rx="8" fill="rgba(0,229,255,0.15)" stroke="var(--acc)" />
            <rect x="20" y="30" width="80" height="16" rx="8" fill="rgba(168,85,247,0.25)" />
            <g opacity=".9">
              <rect x="28" y="48" width="12" height="40" fill="rgba(255,255,255,.12)"/>
              <rect x="44" y="48" width="12" height="40" fill="rgba(255,255,255,.12)"/>
              <rect x="60" y="48" width="12" height="40" fill="rgba(255,255,255,.12)"/>
              <rect x="76" y="48" width="12" height="40" fill="rgba(255,255,255,.12)"/>
            </g>
          </svg>
        </div>
        <div class="text-cyan-100/90 text-sm title-font">Opening ${key.charAt(0).toUpperCase()+key.slice(1)} Crate...</div>
      `;
      beam.style.display = 'block';
      modal.style.display = 'flex';

      setTimeout(()=>{
        const rareRoll = Math.random() < cfg.rare;
        const type = rareRoll ? 'accessory' : pickType(cfg.pool); // Mini has no color in pool
        let value;
        if (type==='color') value = sample(colorsPool);
        if (type==='pattern') value = sample(patternsPool);
        if (type==='accessory') value = rareRoll ? sample(rareAccessories) : sample(commonAccessories);

        const item = { type, value };
        state.inventory.push(item);
        persistWallet();
        renderInventory();

        stage.innerHTML = `
          <div class="reveal neon-border p-4 mx-6 text-center">
            <div class="text-xs text-cyan-200/80 mb-1">${rareRoll ? 'Rare!' : 'You got'}</div>
            <div class="title-font text-lg mb-2">${type.toUpperCase()} — ${String(value).toUpperCase()}</div>
            <div class="flex items-center justify-center mb-3">
              ${type==='color' ? avatarSvg(value,'none',72,state.player.accessory)
                : type==='pattern' ? avatarSvg(state.player.color, value, 72, state.player.accessory)
                : avatarSvg(state.player.color, state.player.pattern, 72, value)}
            </div>
            <div class="flex items-center justify-center gap-2">
              <button id="equipPrize" class="neon-btn px-3 py-2">Equip</button>
              <button id="closePrize" class="neon-btn px-3 py-2 opacity-90">Close</button>
            </div>
          </div>
        `;
        beam.style.display = 'none';

        $('#equipPrize')?.addEventListener('click', ()=>{
          equipItem(item);
          modal.style.display='none';
        });
        $('#closePrize')?.addEventListener('click', ()=>{
          modal.style.display='none';
        });
      }, 1100);
    }

    function renderInventory(){
      const list = $('#inventory');
      if (!list) return;
      list.innerHTML = '';
      if (!state.inventory.length){ list.innerHTML = '<li class="text-cyan-200/70">No items yet. Buy a crate!</li>'; return; }
      state.inventory.forEach((it, idx)=>{
        const li = document.createElement('li');
        li.className = 'neon-border p-2 flex items-center justify-between gap-2';
        li.innerHTML = `<span>${idx+1}. ${it.type} → ${it.value}</span><button class="neon-btn px-3 py-1.5 text-xs">Equip</button>`;
        li.querySelector('button').addEventListener('click', ()=> equipItem(it));
        list.appendChild(li);
      });
    }

    function equipItem(item){
      if (item.type==='pattern') state.player.pattern = item.value;
      if (item.type==='accessory') state.player.accessory = item.value;
      if (item.type==='color') { state.player.color = item.value; applyTheme(item.value); }
      localStorage.setItem('nr-player', JSON.stringify(state.player));
      renderBigAvatar();
      renderMiniAvatar();
      if (state.player.el){
        state.player.el.innerHTML = avatarSvg(state.player.color, state.player.pattern, 28, state.player.accessory);
      }
      toast('Equipped!');
    }

    // Wallet
    function addCoins(n){
      state.coins = Math.max(0, (state.coins||0) + n);
      persistWallet();
    }
    function spendCoins(n){
      if ((state.coins||0) < n) return false;
      state.coins -= n;
      persistWallet();
      return true;
    }
    function persistWallet(){
      localStorage.setItem('nr-wallet', JSON.stringify({ coins: state.coins, inventory: state.inventory }));
    }
    function renderCoins(){
      const val = (state.coins||0);
      ['coinCount','coinCountM','coinCountShop','coinCountPlay'].forEach(id=>{
        const el = document.getElementById(id);
        if (el) el.textContent = val;
      });
    }

    // Maze Game
    function bindMazePlay(){
      document.addEventListener('click', (e)=>{
        if (e.target.id==='startBtn') startGame();
        if (e.target.id==='regenBtn'){ stopGame(); buildMaze(true); }
      });
      document.addEventListener('keydown', onKey);
      window.addEventListener('resize', ()=> {
        if (!state.maze) return;
        layoutMaze();
        placePlayerExit();
      });
    }

    function startGame(){
      state.finished = false;
      buildMaze(); // fresh layout every run
      state.player.c = 0; state.player.r = 0;
      placePlayerExit();
      movePlayerTo(0,0,true);

      state.running = true;
      state.startTime = Date.now();
      const st = $('#status'); if (st) st.textContent = 'Find the exit!';
      const cf = $('#confetti'); if (cf) cf.innerHTML = '';
      const tm = $('#timer'); if (tm) tm.textContent = '00:00';
      const btn = $('#startBtn'); if (btn){ btn.style.opacity = .6; btn.textContent = 'Running...'; }
      tickTimer();
    }

    function stopGame(){
      state.running = false;
      if (state.timerId){ cancelAnimationFrame(state.timerId); state.timerId=null; }
      const btn = $('#startBtn'); if (btn){ btn.style.opacity = 1; btn.textContent = 'Start Round'; }
    }

    function tickTimer(){
      const step = ()=>{
        if (!state.running) return;
        const s = Math.floor((Date.now() - state.startTime)/1000);
        const mm = String(Math.floor(s/60)).padStart(2,'0');
        const ss = String(s%60).toString().padStart(2,'0');
        const tm = $('#timer'); if (tm) tm.textContent = `${mm}:${ss}`;
        state.timerId = requestAnimationFrame(step);
      };
      step();
    }

    // Build perfect maze
    function buildMaze(scrollToTop=false){
      const arena = $('#arena');
      if (!arena) return;
      $$('.maze-wall', arena).forEach(n=>n.remove());
      if (state.player.el) { try{ state.player.el.remove(); }catch{} state.player.el = null; }
      if (state.exit.el) { try{ state.exit.el.remove(); }catch{} state.exit.el = null; }
      const cf = $('#confetti'); if (cf) cf.innerHTML = '';

      if (scrollToTop) arena.scrollTop = 0;

      const W = arena.clientWidth - state.margin*2;
      const H = arena.clientHeight - state.margin*2;

      const factor = state.size===1 ? 1.3 : state.size===3 ? 0.85 : 1.0;
      const baseCell = Math.max(26, Math.min(44, Math.floor(Math.min(W/19, H/11))));
      let cellSize = Math.floor(baseCell * factor);
      if (state.difficulty==='easy') cellSize = Math.floor(cellSize * 1.1);
      if (state.difficulty==='hard') cellSize = Math.max(18, Math.floor(cellSize * 0.85));

      state.cols = Math.max(9, Math.floor(W / cellSize));
      state.rows = Math.max(7, Math.floor(H / cellSize));
      if (state.cols % 2 === 0) state.cols += 1;
      if (state.rows % 2 === 0) state.rows += 1;
      state.cellW = Math.floor(W / state.cols);
      state.cellH = Math.floor(H / state.rows);

      const N=1,E=2,S=4,Ww=8;
      const opp = {1:4,2:8,4:1,8:2};
      const dx = {1:0,2:1,4:0,8:-1};
      const dy = {1:-1,2:0,4:1,8:0};
      const dirs = [N,E,S,Ww];

      const cells = Array.from({length: state.rows}, ()=> Array.from({length: state.cols}, ()=>0));

      carve(0,0);
      function carve(cx, cy){
        const order = dirs.slice().sort(()=> (state.difficulty==='hard' ? Math.random()-0.55 : state.difficulty==='easy' ? Math.random()-0.45 : Math.random()-0.5) );
        for (const d of order){
          const nx = cx + dx[d], ny = cy + dy[d];
          if (ny>=0 && ny<state.rows && nx>=0 && nx<state.cols && cells[ny][nx]===0){
            cells[cy][cx] |= d;
            cells[ny][nx] |= opp[d];
            carve(nx, ny);
          }
        }
      }

      // Safety openings
      if ((cells[0][0] & (E|S)) === 0){
        if (state.cols>1){ cells[0][0] |= E; cells[0][1] |= Ww; }
        else if (state.rows>1){ cells[0][0] |= S; cells[1][0] |= N; }
      }
      if ((cells[state.rows-1][state.cols-1] & (Ww|N)) === 0){
        if (state.cols>1){ cells[state.rows-1][state.cols-1] |= Ww; cells[state.rows-1][state.cols-2] |= E; }
        else if (state.rows>1){ cells[state.rows-2][state.cols-1] |= S; cells[state.rows-1][state.cols-1] |= N; }
      }

      state.maze = { cells };
      layoutMaze();

      state.player.c = 0; state.player.r = 0;
      state.exit.c = state.cols-1; state.exit.r = state.rows-1;
      placePlayerExit();
    }

    function layoutMaze(){
      const arena = $('#arena');
      const { cells } = state.maze || {};
      if (!cells) return;

      const ax = state.margin, ay = state.margin;
      const cw = state.cellW, ch = state.cellH;

      const borders = [
        {x: ax, y: ay, w: cw*state.cols, h: 6},
        {x: ax, y: ay+ch*state.rows-6, w: cw*state.cols, h: 6},
        {x: ax, y: ay, w: 6, h: ch*state.rows},
        {x: ax+cw*state.cols-6, y: ay, w: 6, h: ch*state.rows},
      ];
      borders.forEach(b=> addWall(b.x,b.y,b.w,b.h));

      for (let r=0;r<state.rows;r++){
        for (let c=0;c<state.cols;c++){
          const bit = cells[r][c];
          if ((bit & 2) === 0){
            const x = ax + (c+1)*cw - 3;
            const y = ay + r*ch;
            addWall(x, y, 6, ch);
          }
          if ((bit & 4) === 0){
            const x = ax + c*cw;
            const y = ay + (r+1)*ch - 3;
            addWall(x, y, cw, 6);
          }
        }
      }
      function addWall(x,y,w,h){
        const d = document.createElement('div');
        d.className = 'maze-wall';
        d.style.left = x+'px'; d.style.top = y+'px';
        d.style.width = w+'px'; d.style.height = h+'px';
        arena.appendChild(d);
      }
    }

    function placePlayerExit(){
      const arena = $('#arena');
      const ax = state.margin, ay = state.margin;
      const cw = state.cellW, ch = state.cellH;

      // Exit
      if (!state.exit.el){
        const ex = document.createElement('div');
        ex.className = 'exit';
        state.exit.el = ex;
        arena.appendChild(ex);
      }
      const exCx = ax + state.exit.c*cw + cw/2 - 18;
      const exCy = ay + state.exit.r*ch + ch/2 - 18;
      state.exit.el.style.transform = `translate(${exCx}px, ${exCy}px)`;

      // Player
      if (!state.player.el || !document.body.contains(state.player.el)){
        const p = document.createElement('div');
        p.className = 'player';
        state.player.el = p;
        arena.appendChild(p);
      }
      state.player.el.innerHTML = avatarSvg(state.player.color, state.player.pattern, 28, state.player.accessory);
      movePlayerTo(state.player.c, state.player.r, true);
    }

    function movePlayerTo(c, r, instant=false){
      const ax = state.margin, ay = state.margin;
      const cw = state.cellW, ch = state.cellH;
      const x = ax + c*cw + cw/2 - 17;
      const y = ay + r*ch + ch/2 - 17;
      if (instant){
        state.player.el.style.transition = 'none';
        state.player.el.style.transform = `translate(${x}px, ${y}px)`;
        state.player.el.getBoundingClientRect();
        state.player.el.style.transition = 'transform .12s ease';
      } else {
        state.player.el.style.transform = `translate(${x}px, ${y}px)`;
      }
      if (state.running && !state.finished && c===state.exit.c && r===state.exit.r){
        win();
      }
    }

    function onKey(e){
      const k = e.key.toLowerCase();
      if (['w','a','s','d','arrowup','arrowdown','arrowleft','arrowright'].includes(k)) e.preventDefault();
      const now = Date.now();
      if (now - state.lastMoveAt < state.moveDelay) return;
      state.lastMoveAt = now;

      let dir = null;
      if (k==='w' || k==='arrowup') dir = 'up';
      if (k==='s' || k==='arrowdown') dir = 'down';
      if (k==='a' || k==='arrowleft') dir = 'left';
      if (k==='d' || k==='arrowright') dir = 'right';
      if (!dir) return;

      tryMove(dir);
    }

    function tryMove(dir){
      if (!state.maze) return;
      const { cells } = state.maze;
      const N=1,E=2,S=4,Ww=8;
      const { c, r } = state.player;
      let nc=c, nr=r, needed=0;

      if (dir==='up'){ needed=N; nr=r-1; }
      if (dir==='right'){ needed=E; nc=c+1; }
      if (dir==='down'){ needed=S; nr=r+1; }
      if (dir==='left'){ needed=Ww; nc=c-1; }

      if (nr<0 || nr>=state.rows || nc<0 || nc>=state.cols) return;
      if ((cells[r][c] & needed) !== 0){
        state.player.c = nc;
        state.player.r = nr;
        movePlayerTo(nc, nr);
        state.moveDelay = state.difficulty==='easy' ? 70 : state.difficulty==='hard' ? 120 : 90;
      }
    }

    function win(){
      if (state.finished) return;
      state.finished = true;
      addCoins(1);
      renderCoins();
      const status = $('#status');
      if (status) status.textContent = `Nice, ${state.player.name}! +1 Coin earned. New maze loading...`;
      pourConfetti();
      stopGame();
      setTimeout(()=> { 
        buildMaze();
        const btn = $('#startBtn'); if (btn){ btn.style.opacity = 1; btn.textContent = 'Start Round'; }
      }, 700);
    }

    function pourConfetti(){
      const conf = $('#confetti');
      if (!conf) return;
      conf.innerHTML = '';
      const colors = ['#00e5ff','#a855f7','#22d3ee','#f59e0b','#10b981'];
      for (let i=0;i<80;i++){
        const s = document.createElement('span');
        s.style.left = (Math.random()*90 + 5) + '%';
        s.style.background = colors[Math.floor(Math.random()*colors.length)];
        s.style.animationDelay = (Math.random()*0.6)+'s';
        conf.appendChild(s);
      }
    }

    // Toast
    function toast(msg){
      const t = document.createElement('div');
      t.className = 'fixed bottom-6 left-1/2 -translate-x-1/2 neon-border px-4 py-2 z-50';
      t.textContent = msg;
      document.body.appendChild(t);
      setTimeout(()=> t.remove(), 1200);
    }

    // Avatar SVG
    function avatarSvg(color='#00e5ff', pattern='none', size=120, accessory='none'){
      const uid = Math.random().toString(36).slice(2,7);
      const pid = (name)=> `${name}-${uid}`;
      const patternDefs = {
        stripes: `<pattern id="${pid('pStripes')}" width="10" height="10" patternUnits="userSpaceOnUse">
                    <rect width="10" height="10" fill="${hexToRgba(color,0.2)}"/>
                    <path d="M0 0 L10 10 M-5 5 L5 15 M5 -5 L15 5" stroke="${color}" stroke-width="2" opacity=".45"/>
                  </pattern>`,
        dots: `<pattern id="${pid('pDots')}" width="10" height="10" patternUnits="userSpaceOnUse">
                 <rect width="10" height="10" fill="${hexToRgba(color,0.2)}"/>
                 <circle cx="5" cy="5" r="1.5" fill="${color}" opacity=".55"/>
               </pattern>`,
        hex: `<pattern id="${pid('pHex')}" width="12" height="10.4" patternUnits="userSpaceOnUse">
                <polygon points="6,0 12,3.46 12,10.4 6,13.86 0,10.4 0,3.46" fill="${hexToRgba(color,0.12)}" />
                <polygon points="6,0 12,3.46 12,10.4 6,13.86 0,10.4 0,3.46" fill="none" stroke="${color}" stroke-width="1" opacity=".35"/>
              </pattern>`
      };
      const fillRef = pattern==='none' ? color
        : (pattern==='stripes' ? `url(#${pid('pStripes')})`
        : pattern==='dots' ? `url(#${pid('pDots')})`
        : `url(#${pid('pHex')})`);

      const crown = accessory==='crown' ? `<path d="M26 24 L34 12 L42 24 L50 12 L58 24 L58 30 L26 30 Z" fill="${color}" opacity=".7" />` : '';
      const antenna = accessory==='antenna' ? `<circle cx="54" cy="10" r="4" fill="${color}"/><rect x="52.5" y="10" width="3" height="14" fill="${color}" />` : '';
      const visor = accessory==='visor' ? `<rect x="22" y="34" width="64" height="16" rx="8" fill="${hexToRgba(color, .6)}" stroke="${color}" opacity=".9"/>` : '';
      const halo = accessory==='halo' ? `<ellipse cx="60" cy="12" rx="18" ry="6" fill="none" stroke="${color}" stroke-width="3" opacity=".8"/>` : '';
      const wings = accessory==='wings' ? `
        <g opacity=".9">
          <path d="M10 60 C 22 40, 22 80, 10 60" fill="none" stroke="${color}" stroke-width="3"/>
          <path d="M110 60 C 98 40, 98 80, 110 60" fill="none" stroke="${color}" stroke-width="3"/>
        </g>
      ` : '';

      return `
        <svg width="${size}" height="${size}" viewBox="0 0 120 120" fill="none" aria-label="Character">
          <defs>${pattern==='none' ? '' : patternDefs[pattern]}</defs>
          <g filter="url(#glow-${uid})">
            ${wings}
            <rect x="18" y="24" width="84" height="72" rx="18" fill="${fillRef}" stroke="${color}" opacity=".9"/>
            <rect x="26" y="32" width="68" height="30" rx="12" fill="black" opacity=".25"/>
            <circle cx="48" cy="47" r="6" fill="white"/>
            <circle cx="72" cy="47" r="6" fill="white"/>
            <path d="M44 70c10 10 22 10 32 0" stroke="white" stroke-width="3" stroke-linecap="round" opacity=".9"/>
            ${halo}${antenna}${visor}${crown}
          </g>
          <defs>
            <filter id="glow-${uid}">
              <feDropShadow dx="0" dy="0" stdDeviation="4" flood-color="${color}" flood-opacity=".6"/>
              <feDropShadow dx="0" dy="0" stdDeviation="10" flood-color="${color}" flood-opacity=".25"/>
            </filter>
          </defs>
        </svg>
      `;
    }
  </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'983f5127d6eb2523',t:'MTc1ODY4NTgyMC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
