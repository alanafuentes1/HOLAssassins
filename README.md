[holassassins (2).html](https://github.com/user-attachments/files/26587664/holassassins.2.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>HOLAssassins — The Kill Board</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Crimson+Pro:ital,wght@0,300;0,400;1,300;1,400&family=Share+Tech+Mono&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0a0b;
    --surface: #111114;
    --surface2: #18181d;
    --border: #2a2a35;
    --red: #e63946;
    --red-dim: #7a1a22;
    --gold: #f4a261;
    --gold-dim: #7a4f2a;
    --green: #52b788;
    --text: #e8e8e8;
    --muted: #6b6b7a;
    --accent: #a8dadc;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Crimson Pro', Georgia, serif;
    min-height: 100vh;
    overflow-x: hidden;
  }

  body::before {
    content: '';
    position: fixed; inset: 0;
    background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.03) 2px, rgba(0,0,0,0.03) 4px);
    pointer-events: none; z-index: 1000;
  }

  body::after {
    content: '';
    position: fixed; inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
    pointer-events: none; z-index: 999; opacity: 0.4;
  }

  header {
    position: relative;
    padding: 32px 40px 0;
    border-bottom: 1px solid var(--border);
    overflow: hidden;
  }

  .header-bg {
    position: absolute; inset: 0;
    background: radial-gradient(ellipse at 50% -20%, rgba(230,57,70,0.15) 0%, transparent 60%);
  }

  .logo {
    position: relative;
    display: flex; align-items: center; gap: 20px; margin-bottom: 8px;
  }

  .map-icon { flex-shrink: 0; animation: mapPulse 3s ease-in-out infinite; }

  @keyframes mapPulse {
    0%,100% { filter: drop-shadow(0 0 8px rgba(230,57,70,0.5)); }
    50%      { filter: drop-shadow(0 0 22px rgba(230,57,70,1)); }
  }

  .logo-text { display: flex; flex-direction: column; justify-content: center; }

  .logo-title {
    font-family: 'Bebas Neue', sans-serif;
    font-size: clamp(48px, 7vw, 90px);
    letter-spacing: 4px; line-height: 0.92;
    background: linear-gradient(135deg, #fff 0%, var(--red) 60%, #7a0010 100%);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
  }

  .logo-sub {
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px; color: var(--muted); letter-spacing: 3px; text-transform: uppercase; margin-top: 4px;
  }

  .header-stats {
    display: flex; gap: 40px; padding: 20px 0 24px;
    font-family: 'Share Tech Mono', monospace; font-size: 13px;
  }

  .stat { display: flex; flex-direction: column; gap: 2px; }
  .stat-label { color: var(--muted); font-size: 10px; letter-spacing: 2px; text-transform: uppercase; }
  .stat-value { color: var(--text); font-size: 22px; }
  .stat-value.red   { color: var(--red); }
  .stat-value.gold  { color: var(--gold); }
  .stat-value.green { color: var(--green); }

  .tabs {
    display: flex; gap: 2px; padding: 20px 40px 0;
    border-bottom: 1px solid var(--border); background: var(--surface);
  }

  .tab {
    padding: 10px 24px;
    font-family: 'Bebas Neue', sans-serif; font-size: 18px; letter-spacing: 2px;
    color: var(--muted); cursor: pointer; border-bottom: 2px solid transparent;
    transition: all 0.2s; background: none;
    border-top: none; border-left: none; border-right: none;
  }
  .tab:hover { color: var(--text); }
  .tab.active { color: var(--red); border-bottom-color: var(--red); }

  .content { padding: 32px 40px; max-width: 1100px; }

  /* Kill Feed */
  .kill-feed { display: flex; flex-direction: column; gap: 8px; }

  .kill-entry {
    display: flex; align-items: center; gap: 10px;
    background: var(--surface); border: 1px solid var(--border); border-left: 3px solid var(--red);
    padding: 10px 16px; animation: slideIn 0.3s ease-out; transition: background 0.2s;
  }
  .kill-entry:hover { background: var(--surface2); }

  @keyframes slideIn {
    from { opacity: 0; transform: translateX(-10px); }
    to   { opacity: 1; transform: translateX(0); }
  }

  .kill-num { font-family: 'Share Tech Mono', monospace; font-size: 11px; color: var(--muted); width: 26px; flex-shrink: 0; }

  .k-avatar {
    width: 36px; height: 36px; border-radius: 50%; object-fit: cover;
    border: 1px solid var(--border); background: var(--surface2); flex-shrink: 0;
    display: flex; align-items: center; justify-content: center;
    font-family: 'Bebas Neue', sans-serif; font-size: 15px; color: var(--muted);
    overflow: hidden;
  }
  .k-avatar img { width: 100%; height: 100%; object-fit: cover; border-radius: 50%; }

  .kill-killer { font-size: 17px; color: var(--text); flex: 1; min-width: 80px; }
  .kill-arrow  { font-size: 18px; color: var(--red); flex-shrink: 0; }
  .kill-victim { font-size: 17px; color: var(--muted); text-decoration: line-through; text-decoration-color: var(--red); flex: 1; min-width: 80px; }

  .kill-time {
    font-family: 'Share Tech Mono', monospace; font-size: 11px; color: var(--muted);
    flex-shrink: 0; min-width: 72px; text-align: right;
  }

  .kill-delete {
    background: none; border: none; color: var(--muted); cursor: pointer;
    font-size: 15px; padding: 0; opacity: 0; transition: opacity 0.2s, color 0.2s; flex-shrink: 0;
  }
  .kill-entry:hover .kill-delete { opacity: 1; }
  .kill-delete:hover { color: var(--red); }

  /* Leaderboard */
  .leaderboard { display: flex; flex-direction: column; gap: 6px; }

  .lb-header {
    display: grid; grid-template-columns: 40px 42px 1fr 80px 80px 110px;
    padding: 8px 16px;
    font-family: 'Share Tech Mono', monospace; font-size: 10px;
    letter-spacing: 2px; text-transform: uppercase; color: var(--muted);
    border-bottom: 1px solid var(--border);
  }

  .lb-row {
    display: grid; grid-template-columns: 40px 42px 1fr 80px 80px 110px;
    padding: 10px 16px; background: var(--surface); border: 1px solid var(--border);
    align-items: center; transition: background 0.2s; animation: slideIn 0.3s ease-out;
  }
  .lb-row:hover { background: var(--surface2); }
  .lb-row.dead  { opacity: 0.45; border-left: 3px solid var(--red-dim); }

  .lb-rank { font-family: 'Bebas Neue', sans-serif; font-size: 22px; color: var(--muted); }
  .lb-rank.top1 { color: var(--gold); }
  .lb-rank.top2 { color: #c0c0c0; }
  .lb-rank.top3 { color: #cd7f32; }

  .lb-photo-cell {
    width: 32px; height: 32px; border-radius: 50%;
    border: 1px solid var(--border); background: var(--surface2);
    display: flex; align-items: center; justify-content: center;
    font-family: 'Bebas Neue', sans-serif; font-size: 14px; color: var(--muted);
    overflow: hidden;
  }
  .lb-photo-cell img { width: 100%; height: 100%; object-fit: cover; border-radius: 50%; }

  .lb-name { font-size: 17px; }
  .lb-name .dead-tag { font-family: 'Share Tech Mono', monospace; font-size: 10px; color: var(--red); letter-spacing: 1px; margin-left: 8px; vertical-align: middle; }
  .lb-kills  { font-family: 'Share Tech Mono', monospace; font-size: 20px; color: var(--green); text-align: center; }
  .lb-status { text-align: center; }
  .badge-alive { font-family: 'Share Tech Mono', monospace; font-size: 10px; color: var(--green); border: 1px solid var(--green); padding: 3px 8px; letter-spacing: 1px; }
  .badge-dead  { font-family: 'Share Tech Mono', monospace; font-size: 10px; color: var(--red);   border: 1px solid var(--red-dim); padding: 3px 8px; letter-spacing: 1px; }
  .lb-target   { font-family: 'Share Tech Mono', monospace; font-size: 11px; color: var(--gold); text-align: right; }

  /* Players */
  .players-grid {
    display: grid; grid-template-columns: repeat(auto-fill, minmax(165px, 1fr));
    gap: 12px; margin-bottom: 24px;
  }

  .player-card {
    background: var(--surface); border: 1px solid var(--border);
    padding: 18px 14px 14px; position: relative; transition: all 0.2s;
    display: flex; flex-direction: column; align-items: center; gap: 8px;
  }
  .player-card.dead { border-color: var(--red-dim); background: rgba(122,26,34,0.1); }

  .player-photo-wrap {
    position: relative; width: 84px; height: 84px; cursor: pointer;
  }

  .player-photo-inner {
    width: 84px; height: 84px; border-radius: 50%;
    border: 2px solid var(--border); background: var(--surface2);
    display: flex; align-items: center; justify-content: center;
    font-family: 'Bebas Neue', sans-serif; font-size: 32px; color: var(--muted);
    overflow: hidden; transition: border-color 0.2s;
  }
  .player-photo-inner img { width: 100%; height: 100%; object-fit: cover; border-radius: 50%; }
  .player-photo-wrap:hover .player-photo-inner { border-color: var(--red); }

  .photo-upload-hint {
    position: absolute; inset: 0; border-radius: 50%;
    background: rgba(0,0,0,0.6);
    display: flex; align-items: center; justify-content: center;
    font-size: 10px; color: #fff; font-family: 'Share Tech Mono', monospace;
    letter-spacing: 1px; text-align: center; line-height: 1.4;
    opacity: 0; transition: opacity 0.2s;
  }
  .player-photo-wrap:hover .photo-upload-hint { opacity: 1; }

  .dead-overlay {
    position: absolute; inset: 0; border-radius: 50%;
    background: rgba(122,26,34,0.6);
    display: flex; align-items: center; justify-content: center; font-size: 30px;
  }

  .photo-file-input { display: none; }

  .p-name   { font-size: 16px; text-align: center; }
  .p-kills  { font-family: 'Share Tech Mono', monospace; font-size: 12px; color: var(--green); }
  .p-target { font-family: 'Share Tech Mono', monospace; font-size: 10px; color: var(--gold); text-align: center; }

  /* Forms */
  .panel { background: var(--surface); border: 1px solid var(--border); padding: 24px; margin-bottom: 24px; }

  .panel-title {
    font-family: 'Bebas Neue', sans-serif; font-size: 22px; letter-spacing: 3px; color: var(--red);
    margin-bottom: 16px; display: flex; align-items: center; gap: 10px;
  }
  .panel-title::after { content: ''; flex: 1; height: 1px; background: var(--border); }

  .form-row  { display: flex; gap: 10px; flex-wrap: wrap; }
  .form-group { display: flex; flex-direction: column; gap: 6px; flex: 1; min-width: 140px; }

  label { font-family: 'Share Tech Mono', monospace; font-size: 10px; letter-spacing: 2px; text-transform: uppercase; color: var(--muted); }

  input, select {
    background: var(--bg); border: 1px solid var(--border); color: var(--text);
    padding: 10px 14px; font-family: 'Crimson Pro', Georgia, serif; font-size: 16px;
    outline: none; transition: border-color 0.2s; width: 100%;
  }
  input:focus, select:focus { border-color: var(--red); }
  select option { background: var(--bg); }

  .btn {
    padding: 11px 24px; font-family: 'Bebas Neue', sans-serif;
    font-size: 18px; letter-spacing: 2px; cursor: pointer; border: none;
    transition: all 0.15s; align-self: flex-end;
  }
  .btn-red     { background: var(--red); color: #fff; }
  .btn-red:hover { background: #c1121f; }
  .btn-outline { background: transparent; border: 1px solid var(--border); color: var(--muted); }
  .btn-outline:hover { border-color: var(--red); color: var(--text); }
  .btn-small   { padding: 7px 14px; font-size: 13px; }

  .tab-panel { display: none; }
  .tab-panel.active { display: block; }

  /* Target chain */
  .chain-container {
    display: flex; flex-wrap: wrap; gap: 6px; align-items: center;
    font-family: 'Share Tech Mono', monospace; font-size: 12px;
    padding: 16px; background: var(--bg); border: 1px solid var(--border); margin-top: 8px;
  }
  .chain-player { padding: 4px 10px; border: 1px solid var(--border); color: var(--text); }
  .chain-player.dead { border-color: var(--red-dim); color: var(--muted); text-decoration: line-through; }
  .chain-arrow  { color: var(--gold); font-size: 14px; }

  .empty { text-align: center; padding: 60px 20px; color: var(--muted); font-style: italic; font-size: 18px; }
  .empty-icon { font-size: 48px; display: block; margin-bottom: 12px; opacity: 0.3; }

  #toast {
    position: fixed; bottom: 32px; right: 32px;
    background: var(--red); color: #fff; padding: 12px 20px;
    font-family: 'Share Tech Mono', monospace; font-size: 13px; letter-spacing: 1px;
    z-index: 9999; transform: translateY(80px); opacity: 0; transition: all 0.3s; max-width: 300px;
  }
  #toast.show { transform: translateY(0); opacity: 1; }

  @media (max-width: 700px) {
    header, .tabs, .content { padding-left: 16px; padding-right: 16px; }
    .header-stats { gap: 20px; flex-wrap: wrap; }
    .lb-header, .lb-row { grid-template-columns: 32px 36px 1fr 56px 66px; }
    .lb-target { display: none; }
    .kill-time { display: none; }
  }
</style>
</head>
<body>

<header>
  <div class="header-bg"></div>
  <div class="logo">

    <!-- South America + Caribbean — clean accurate SVG -->
    <svg class="map-icon" width="68" height="90" viewBox="0 0 340 450" xmlns="http://www.w3.org/2000/svg">
      <defs>
        <linearGradient id="lg" x1="0%" y1="0%" x2="100%" y2="100%">
          <stop offset="0%" stop-color="#ff8088"/>
          <stop offset="100%" stop-color="#900010"/>
        </linearGradient>
        <linearGradient id="ig" x1="0%" y1="0%" x2="100%" y2="100%">
          <stop offset="0%" stop-color="#ffc08a"/>
          <stop offset="100%" stop-color="#b05820"/>
        </linearGradient>
      </defs>

      <!-- ── Caribbean islands ── -->
      <!-- Cuba -->
      <path fill="url(#ig)" d="M58,28 C70,22 90,20 108,23 C126,26 138,33 140,40 C138,44 128,46 114,44 C100,42 82,40 68,42 C56,44 48,40 48,35 C48,31 52,30 58,28 Z"/>
      <!-- Hispaniola -->
      <path fill="url(#ig)" d="M152,36 C162,31 178,30 190,34 C200,38 204,45 200,50 C194,54 180,54 168,50 C156,46 148,42 152,36 Z"/>
      <!-- Puerto Rico -->
      <ellipse fill="url(#ig)" cx="214" cy="52" rx="16" ry="7" transform="rotate(-5,214,52)"/>
      <!-- Jamaica -->
      <ellipse fill="url(#ig)" cx="122" cy="52" rx="13" ry="6" transform="rotate(-10,122,52)"/>
      <!-- Lesser Antilles (arc from ~220,68 curving SE) -->
      <circle fill="url(#ig)" cx="228" cy="66" r="5"/>
      <circle fill="url(#ig)" cx="236" cy="80" r="4.5"/>
      <circle fill="url(#ig)" cx="242" cy="95" r="4"/>
      <circle fill="url(#ig)" cx="246" cy="110" r="3.5"/>
      <circle fill="url(#ig)" cx="248" cy="124" r="3.5"/>
      <circle fill="url(#ig)" cx="248" cy="138" r="3"/>
      <!-- Trinidad & Tobago -->
      <ellipse fill="url(#ig)" cx="240" cy="154" rx="9" ry="6" transform="rotate(10,240,154)"/>
      <!-- Bahamas cluster -->
      <ellipse fill="url(#ig)" cx="140" cy="18" rx="9" ry="3" transform="rotate(-18,140,18)"/>
      <ellipse fill="url(#ig)" cx="158" cy="12" rx="6" ry="2.5" transform="rotate(-12,158,12)"/>
      <ellipse fill="url(#ig)" cx="172" cy="8"  rx="4" ry="2"   transform="rotate(-8,172,8)"/>

      <!-- ── South America mainland ── -->
      <!--
        Real outline, roughly traced from geographic data:
        Top (Venezuela/Colombia/Guyana) coast runs ~x80-230, y100
        Brazil's eastern "nose" bulges to ~x260, y230
        Argentina/Chile taper to a point ~x155, y445
      -->
      <path fill="url(#lg)" d="
        M 148,102
        C 136,97  122,95  108,97
        C 94,99   80,104  70,112
        C 60,120  54,132  50,144
        C 46,156  46,168  48,180
        C 50,192  54,202  56,212
        C 58,222  56,232  52,240
        C 48,248  44,256  42,264
        C 40,274  40,284  44,294
        C 48,304  54,312  58,322
        C 62,334  62,346  64,358
        C 66,370  70,381  76,390
        C 82,399  90,406  96,413
        C 102,420 106,428 108,435
        C 110,441 112,445 116,447
        C 120,449 124,448 128,445
        C 132,442 134,436 134,428
        C 134,420 132,412 132,404
        C 132,396 134,388 138,382
        C 142,376 148,372 154,368
        C 160,364 166,360 170,354
        C 174,348 176,340 176,332
        C 176,324 174,316 174,308
        C 174,300 176,292 180,286
        C 184,280 190,276 194,270
        C 198,264 200,256 200,248
        C 200,240 198,232 198,224
        C 198,216 200,208 204,202
        C 210,194 218,190 224,184
        C 230,178 234,170 234,162
        C 234,154 230,146 226,140
        C 222,134 216,130 214,124
        C 212,116 214,108 212,102
        C 208,96  200,94  194,96
        C 188,98  182,104 176,106
        C 170,108 162,107 156,105
        C 152,103 150,102 148,102
        Z
      "/>
      <!-- Brazil eastern bulge -->
      <path fill="url(#lg)" d="
        M 212,102 C 218,98 226,96 232,100
        C 238,104 242,112 244,122
        C 246,132 244,142 240,150
        C 236,158 230,163 224,162
        C 220,160 216,154 214,146
        C 212,138 212,128 212,118
        C 212,110 212,104 212,102 Z
      " opacity="0.9"/>
      <!-- Patagonia tip -->
      <path fill="url(#lg)" d="
        M 116,447 C 118,450 122,452 126,450
        C 130,448 132,443 130,439
        C 128,436 122,436 118,439
        C 115,441 114,445 116,447 Z
      " opacity="0.8"/>
    </svg>

    <div class="logo-text">
      <div class="logo-title">HOLAssassins</div>
      <div class="logo-sub">Let the Games Begin &nbsp;|&nbsp; Classified</div>
    </div>
  </div>

  <div class="header-stats">
    <div class="stat">
      <span class="stat-label">Players Alive</span>
      <span class="stat-value green" id="stat-alive">0</span>
    </div>
    <div class="stat">
      <span class="stat-label">Eliminated</span>
      <span class="stat-value red" id="stat-dead">0</span>
    </div>
    <div class="stat">
      <span class="stat-label">Total Kills</span>
      <span class="stat-value" id="stat-kills">0</span>
    </div>
    <div class="stat">
      <span class="stat-label">Top Assassin</span>
      <span class="stat-value gold" id="stat-top">—</span>
    </div>
  </div>
</header>

<div class="tabs">
  <button class="tab active" onclick="switchTab('kills',this)">Kill Feed</button>
  <button class="tab" onclick="switchTab('leaderboard',this)">Leaderboard</button>
  <button class="tab" onclick="switchTab('players',this)">Players</button>
  <button class="tab" onclick="switchTab('targets',this)">Target Chain</button>
</div>

<div class="content">

  <!-- KILL FEED -->
  <div class="tab-panel active" id="tab-kills">
    <div class="panel">
      <div class="panel-title">Log a Kill</div>
      <div class="form-row">
        <div class="form-group">
          <label>Assassin</label>
          <select id="kill-killer"><option value="">— Select Killer —</option></select>
        </div>
        <div class="form-group">
          <label>Victim</label>
          <select id="kill-victim"><option value="">— Select Victim —</option></select>
        </div>
        <button class="btn btn-red" onclick="logKill()">🎯 LOG KILL</button>
      </div>
    </div>
    <div id="kill-feed" class="kill-feed">
      <div class="empty"><span class="empty-icon">🎯</span>No kills yet. The hunt begins…</div>
    </div>
  </div>

  <!-- LEADERBOARD -->
  <div class="tab-panel" id="tab-leaderboard">
    <div class="lb-header">
      <div>#</div><div></div><div>Name</div>
      <div style="text-align:center">Kills</div>
      <div style="text-align:center">Status</div>
      <div style="text-align:right">Target</div>
    </div>
    <div id="leaderboard-list" class="leaderboard">
      <div class="empty"><span class="empty-icon">🏆</span>Add players to begin</div>
    </div>
  </div>

  <!-- PLAYERS -->
  <div class="tab-panel" id="tab-players">
    <div class="panel">
      <div class="panel-title">Add Player</div>
      <div class="form-row">
        <div class="form-group" style="flex:2">
          <label>Player Name</label>
          <input type="text" id="player-name" placeholder="Full name or nickname" onkeydown="if(event.key==='Enter')addPlayer()">
        </div>
        <button class="btn btn-red" onclick="addPlayer()">+ ADD</button>
      </div>
    </div>
    <div id="players-grid" class="players-grid">
      <div class="empty" style="grid-column:1/-1"><span class="empty-icon">👤</span>No players added yet</div>
    </div>
  </div>

  <!-- TARGET CHAIN -->
  <div class="tab-panel" id="tab-targets">
    <div class="panel">
      <div class="panel-title">Assign Targets</div>
      <p style="font-size:15px;color:var(--muted);margin-bottom:14px;font-style:italic">Shuffle assigns each alive player a random target in a circular chain.</p>
      <div class="form-row">
        <button class="btn btn-red" onclick="shuffleTargets()">🔀 SHUFFLE TARGETS</button>
        <button class="btn btn-outline" onclick="clearTargets()">Clear Targets</button>
      </div>
      <div id="target-chain" class="chain-container">
        <span style="color:var(--muted);font-style:italic;font-size:13px">No target chain assigned</span>
      </div>
    </div>
    <div class="panel">
      <div class="panel-title">Manual Assignment</div>
      <div class="form-row">
        <div class="form-group">
          <label>Player</label>
          <select id="target-player"><option value="">— Player —</option></select>
        </div>
        <div class="form-group">
          <label>Their Target</label>
          <select id="target-target"><option value="">— Target —</option></select>
        </div>
        <button class="btn btn-outline btn-small" style="align-self:flex-end" onclick="assignTarget()">Assign</button>
      </div>
    </div>
  </div>

</div>

<div id="toast"></div>

<script>
// ── State ──────────────────────────────────────────────────────────────────────
let state = { players: [], kills: [] };

function loadState() {
  try { const s = localStorage.getItem('holassassins_v3'); if (s) state = JSON.parse(s); } catch(e) {}
}
function saveState() { localStorage.setItem('holassassins_v3', JSON.stringify(state)); }
function uid() { return Date.now().toString(36) + Math.random().toString(36).slice(2,6); }

// ── Toast ──────────────────────────────────────────────────────────────────────
function toast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg; t.classList.add('show');
  clearTimeout(t._t); t._t = setTimeout(() => t.classList.remove('show'), 2600);
}

// ── Tabs ───────────────────────────────────────────────────────────────────────
function switchTab(name, btn) {
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  document.querySelectorAll('.tab-panel').forEach(p => p.classList.remove('active'));
  btn.classList.add('active');
  document.getElementById('tab-'+name).classList.add('active');
  render();
}

// ── Photo upload ───────────────────────────────────────────────────────────────
function triggerPhoto(id) {
  const el = document.getElementById('fi-'+id);
  if (el) el.click();
}
function handlePhoto(id, input) {
  const file = input.files[0]; if (!file) return;
  const reader = new FileReader();
  reader.onload = e => {
    const p = state.players.find(p => p.id === id);
    if (p) { p.photo = e.target.result; saveState(); render(); }
  };
  reader.readAsDataURL(file);
}

// ── Players ────────────────────────────────────────────────────────────────────
function addPlayer() {
  const inp = document.getElementById('player-name');
  const name = inp.value.trim();
  if (!name) return toast('Enter a player name');
  if (state.players.find(p => p.name.toLowerCase() === name.toLowerCase())) return toast('Player already exists');
  state.players.push({ id: uid(), name, alive: true, kills: 0, target: null, photo: null });
  inp.value = ''; saveState(); render();
  toast(name + ' has entered the game');
}

function removePlayer(id) {
  const p = state.players.find(p => p.id === id);
  if (!confirm('Remove ' + (p?.name || 'player') + '?')) return;
  state.players = state.players.filter(p => p.id !== id);
  state.kills   = state.kills.filter(k => k.killerId !== id && k.victimId !== id);
  state.players.forEach(p => { if (p.target === id) p.target = null; });
  saveState(); render(); toast('Player removed');
}

// ── Kills ──────────────────────────────────────────────────────────────────────
function logKill() {
  const killerId = document.getElementById('kill-killer').value;
  const victimId = document.getElementById('kill-victim').value;
  if (!killerId) return toast('Select the assassin');
  if (!victimId) return toast('Select the victim');
  if (killerId === victimId) return toast("Can't kill yourself");
  const killer = state.players.find(p => p.id === killerId);
  const victim = state.players.find(p => p.id === victimId);
  if (!killer || !victim) return toast('Invalid players');
  if (!victim.alive) return toast(victim.name + ' is already dead');
  if (!killer.alive) return toast(killer.name + ' is dead — ghosts can\'t kill');

  victim.alive = false;
  killer.kills += 1;
  if (victim.target && victim.target !== killerId) killer.target = victim.target;
  else if (killer.target === victimId) killer.target = null;

  state.kills.push({ id: uid(), killerId, victimId, timestamp: new Date().toISOString() });
  saveState(); render();
  toast('☠️ ' + killer.name + ' eliminated ' + victim.name + '!');
}

function deleteKill(id) {
  const kill = state.kills.find(k => k.id === id);
  if (!kill) return;
  if (!confirm('Undo this kill? The victim will be revived.')) return;
  const victim = state.players.find(p => p.id === kill.victimId);
  const killer = state.players.find(p => p.id === kill.killerId);
  if (victim) victim.alive = true;
  if (killer) killer.kills = Math.max(0, killer.kills - 1);
  state.kills = state.kills.filter(k => k.id !== id);
  saveState(); render(); toast('Kill undone — player revived');
}

// ── Targets ────────────────────────────────────────────────────────────────────
function shuffleTargets() {
  const alive = state.players.filter(p => p.alive);
  if (alive.length < 2) return toast('Need at least 2 alive players');
  const sh = [...alive].sort(() => Math.random() - 0.5);
  sh.forEach((p, i) => { p.target = sh[(i+1) % sh.length].id; });
  state.players.filter(p => !p.alive).forEach(p => p.target = null);
  saveState(); render(); toast('Targets shuffled! 🔀');
}

function clearTargets() {
  state.players.forEach(p => p.target = null);
  saveState(); render(); toast('All targets cleared');
}

function assignTarget() {
  const pid = document.getElementById('target-player').value;
  const tid = document.getElementById('target-target').value;
  if (!pid || !tid) return toast('Select both player and target');
  if (pid === tid) return toast("Can't target yourself");
  const p = state.players.find(p => p.id === pid);
  if (p) p.target = tid;
  saveState(); render(); toast('Target assigned');
}

// ── Render ─────────────────────────────────────────────────────────────────────
function render() {
  renderStats(); renderSelects(); renderKillFeed(); renderLeaderboard(); renderPlayers(); renderTargetChain();
}

function renderStats() {
  const alive = state.players.filter(p => p.alive).length;
  const dead  = state.players.filter(p => !p.alive).length;
  const top   = [...state.players].sort((a,b) => b.kills - a.kills)[0];
  document.getElementById('stat-alive').textContent = alive;
  document.getElementById('stat-dead').textContent  = dead;
  document.getElementById('stat-kills').textContent = state.kills.length;
  document.getElementById('stat-top').textContent   = top?.kills > 0 ? top.name : '—';
}

function renderSelects() {
  const alive = state.players.filter(p => p.alive);
  const opts = (arr, ph) => '<option value="">'+ph+'</option>' +
    arr.map(p => '<option value="'+p.id+'">'+escHtml(p.name)+'</option>').join('');
  document.getElementById('kill-killer').innerHTML   = opts(alive,'— Select Killer —');
  document.getElementById('kill-victim').innerHTML   = opts(alive,'— Select Victim —');
  document.getElementById('target-player').innerHTML = opts(alive,'— Player —');
  document.getElementById('target-target').innerHTML = opts(alive,'— Target —');
}

function photoDiv(p, size) {
  const s = size || 36;
  if (p && p.photo) {
    return '<div class="k-avatar"><img src="'+p.photo+'" alt="'+escHtml(p.name)+'"></div>';
  }
  return '<div class="k-avatar" style="width:'+s+'px;height:'+s+'px;font-size:'+Math.round(s*0.42)+'px">'+(p?escHtml(p.name[0].toUpperCase()):'?')+'</div>';
}

function renderKillFeed() {
  const feed = document.getElementById('kill-feed');
  if (!state.kills.length) {
    feed.innerHTML = '<div class="empty"><span class="empty-icon">🎯</span>No kills yet. The hunt begins…</div>';
    return;
  }
  feed.innerHTML = [...state.kills].reverse().map((k, i) => {
    const killer = state.players.find(p => p.id === k.killerId);
    const victim = state.players.find(p => p.id === k.victimId);
    return '<div class="kill-entry">'
      + '<span class="kill-num">#'+(state.kills.length-i)+'</span>'
      + photoDiv(killer)
      + '<span class="kill-killer">'+(killer?escHtml(killer.name):'???')+'</span>'
      + '<span class="kill-arrow">→</span>'
      + photoDiv(victim)
      + '<span class="kill-victim">'+(victim?escHtml(victim.name):'???')+'</span>'
      + '<span class="kill-time">'+formatTime(k.timestamp)+'</span>'
      + '<button class="kill-delete" onclick="deleteKill(\''+k.id+'\')" title="Undo">✕</button>'
      + '</div>';
  }).join('');
}

function renderLeaderboard() {
  const list = document.getElementById('leaderboard-list');
  if (!state.players.length) {
    list.innerHTML = '<div class="empty"><span class="empty-icon">🏆</span>Add players to begin</div>';
    return;
  }
  const sorted = [...state.players].sort((a,b) => { if (a.alive!==b.alive) return b.alive-a.alive; return b.kills-a.kills; });
  list.innerHTML = sorted.map((p, i) => {
    const rank = i+1;
    const rc = rank===1?'top1':rank===2?'top2':rank===3?'top3':'';
    const target = state.players.find(t => t.id === p.target);
    const photoCell = p.photo
      ? '<div class="lb-photo-cell"><img src="'+p.photo+'" alt="'+escHtml(p.name)+'"></div>'
      : '<div class="lb-photo-cell">'+escHtml(p.name[0].toUpperCase())+'</div>';
    return '<div class="lb-row '+(p.alive?'':'dead')+'">'
      + '<span class="lb-rank '+rc+'">'+rank+'</span>'
      + photoCell
      + '<span class="lb-name">'+escHtml(p.name)+(!p.alive?'<span class="dead-tag">☠ OUT</span>':'')+'</span>'
      + '<span class="lb-kills">'+p.kills+'</span>'
      + '<span class="lb-status">'+(p.alive?'<span class="badge-alive">ALIVE</span>':'<span class="badge-dead">DEAD</span>')+'</span>'
      + '<span class="lb-target">'+(target?'🎯 '+escHtml(target.name):'—')+'</span>'
      + '</div>';
  }).join('');
}

function renderPlayers() {
  const grid = document.getElementById('players-grid');
  if (!state.players.length) {
    grid.innerHTML = '<div class="empty" style="grid-column:1/-1"><span class="empty-icon">👤</span>No players added yet</div>';
    return;
  }
  const sorted = [...state.players].sort((a,b) => a.name.localeCompare(b.name));
  grid.innerHTML = sorted.map(p => {
    const target = state.players.find(t => t.id === p.target);
    const inner = p.photo
      ? '<img src="'+p.photo+'" alt="'+escHtml(p.name)+'" style="width:100%;height:100%;object-fit:cover;border-radius:50%">'
      : escHtml(p.name[0].toUpperCase());
    return '<div class="player-card '+(p.alive?'':'dead')+'">'
      + '<div class="player-photo-wrap" onclick="triggerPhoto(\''+p.id+'\')">'
      + '  <div class="player-photo-inner">'+inner+'</div>'
      + '  <div class="photo-upload-hint">TAP TO<br>UPLOAD</div>'
      + (!p.alive?'<div class="dead-overlay">💀</div>':'')
      + '</div>'
      + '<input class="photo-file-input" id="fi-'+p.id+'" type="file" accept="image/*" onchange="handlePhoto(\''+p.id+'\',this)">'
      + '<div class="p-name">'+escHtml(p.name)+'</div>'
      + '<div class="p-kills">'+p.kills+' kill'+(p.kills!==1?'s':'')+'</div>'
      + (target?'<div class="p-target">→ '+escHtml(target.name)+'</div>':'')
      + '<button class="btn btn-outline btn-small" style="font-size:11px;padding:4px 10px;margin-top:6px" onclick="removePlayer(\''+p.id+'\')">Remove</button>'
      + '</div>';
  }).join('');
}

function renderTargetChain() {
  const chain = document.getElementById('target-chain');
  const alive = state.players.filter(p => p.alive && p.target);
  if (!alive.length) {
    chain.innerHTML = '<span style="color:var(--muted);font-style:italic;font-size:13px">No target chain assigned</span>';
    return;
  }
  const visited = new Set();
  let current = alive[0];
  const ordered = [];
  while (current && !visited.has(current.id)) {
    visited.add(current.id); ordered.push(current);
    current = state.players.find(p => p.id === current.target && p.alive);
  }
  const unlinked = state.players.filter(p => p.alive && !visited.has(p.id));
  chain.innerHTML = ordered.map((p, i) => {
    const isLast = i === ordered.length - 1;
    return '<span class="chain-player">'+escHtml(p.name)+'</span>'+(isLast?'<span class="chain-arrow">↩</span>':'<span class="chain-arrow">→</span>');
  }).join('')
  + (unlinked.length ? '<br><span style="color:var(--muted);margin-top:8px;display:block;font-size:11px">Unlinked: '+unlinked.map(p=>escHtml(p.name)).join(', ')+'</span>' : '');
}

// ── Helpers ────────────────────────────────────────────────────────────────────
function escHtml(s) {
  return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}
function formatTime(iso) {
  const d = new Date(iso), diff = new Date() - d;
  if (diff < 60000)    return 'just now';
  if (diff < 3600000)  return Math.floor(diff/60000)+'m ago';
  if (diff < 86400000) return Math.floor(diff/3600000)+'h ago';
  return d.toLocaleDateString('en-US',{month:'short',day:'numeric'});
}

loadState();
render();
</script>
</body>
</html>
