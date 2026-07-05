// https://voluble-piroshki-b19004.netlify.app/
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ArogyaGrid — AI for District Health Centres</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,500;9..144,600;9..144,700&family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#FAFCFB;
    --panel:#FFFFFF;
    --teal-deep:#0A3D3A;
    --teal:#0F5C56;
    --teal-tint:#E8F2F0;
    --teal-tint2:#DCEEEB;
    --amber:#C97A1E;
    --amber-tint:#FBF0DC;
    --coral:#C0392B;
    --coral-tint:#FBE7E4;
    --ink:#12302C;
    --muted:#5B6B69;
    --line:rgba(18,48,44,0.10);
  }
  *{box-sizing:border-box; margin:0; padding:0;}
  html{scroll-behavior:smooth;}
  body{
    background:var(--bg); color:var(--ink); font-family:'Inter', sans-serif;
    -webkit-font-smoothing:antialiased;
  }
  .display{font-family:'Fraunces', serif;}
  .mono{font-family:'JetBrains Mono', monospace;}
  a{color:inherit;}

  /* NAV */
  nav{
    position:sticky; top:0; z-index:50; display:flex; align-items:center; justify-content:space-between;
    padding:18px 6%; background:rgba(250,252,251,0.9); backdrop-filter:blur(10px);
    border-bottom:1px solid var(--line);
  }
  .logo{display:flex; align-items:center; gap:10px; font-family:'Fraunces'; font-weight:700; font-size:1.2rem; color:var(--teal-deep);}
  .logo-mark{width:30px; height:30px; border-radius:8px; background:var(--teal); display:flex; align-items:center; justify-content:center;}
  .logo-mark svg{width:16px; height:16px;}
  nav ul{display:flex; gap:30px; list-style:none;}
  nav a{font-size:0.92rem; color:var(--muted); text-decoration:none; font-weight:500; transition:color .2s;}
  nav a:hover{color:var(--teal);}
  .nav-cta{
    background:var(--teal); color:#fff; padding:9px 18px; border-radius:6px; font-size:0.88rem; font-weight:600;
    text-decoration:none; transition:transform .2s;
  }
  .nav-cta:hover{transform:translateY(-1px);}

  section{padding:90px 6%;}
  .eyebrow{
    font-family:'JetBrains Mono'; font-size:0.75rem; letter-spacing:0.12em; text-transform:uppercase;
    color:var(--amber); font-weight:500; display:flex; align-items:center; gap:10px; margin-bottom:16px;
  }
  .eyebrow::before{content:''; width:22px; height:1px; background:var(--amber);}
  h1,h2{font-family:'Fraunces'; letter-spacing:-0.01em; color:var(--teal-deep);}
  h2{font-size:clamp(1.7rem,3vw,2.3rem); font-weight:600; margin-bottom:14px;}

  /* HERO */
  .hero{
    padding:70px 6% 40px; display:grid; grid-template-columns:1.1fr 1fr; gap:50px; align-items:center;
  }
  .hero h1{font-size:clamp(2.4rem,4.2vw,3.6rem); font-weight:700; line-height:1.08; margin-bottom:22px;}
  .hero h1 em{font-style:normal; color:var(--teal);}
  .hero p{color:var(--muted); font-size:1.08rem; line-height:1.65; max-width:520px; margin-bottom:30px;}
  .btn-row{display:flex; gap:14px; flex-wrap:wrap;}
  .btn{
    font-weight:600; font-size:0.95rem; padding:13px 26px; border-radius:6px; text-decoration:none;
    cursor:pointer; border:none; transition:transform .2s, box-shadow .2s; font-family:'Inter';
  }
  .btn-primary{background:var(--teal); color:#fff;}
  .btn-primary:hover{transform:translateY(-2px); box-shadow:0 10px 24px rgba(15,92,86,0.25);}
  .btn-ghost{background:transparent; color:var(--teal-deep); border:1px solid var(--line);}
  .btn-ghost:hover{border-color:var(--teal);}

  /* DISTRICT PULSE MAP - signature element */
  .pulse-card{
    background:var(--teal-deep); border-radius:16px; padding:28px; position:relative; overflow:hidden;
    box-shadow:0 20px 50px rgba(10,61,58,0.25);
  }
  .pulse-head{display:flex; justify-content:space-between; align-items:center; margin-bottom:16px; position:relative; z-index:2;}
  .pulse-head span{font-family:'JetBrains Mono'; font-size:0.75rem; color:#9FCFC8; letter-spacing:0.06em;}
  .pulse-legend{display:flex; gap:14px; font-size:0.7rem; color:#BFE0DB; font-family:'JetBrains Mono';}
  .pulse-legend i{display:inline-block; width:8px; height:8px; border-radius:50%; margin-right:5px;}
  #districtMap{width:100%; height:260px; position:relative; z-index:2;}
  .node{position:absolute; cursor:pointer; transform:translate(-50%,-50%);}
  .node-dot{width:14px; height:14px; border-radius:50%; position:relative;}
  .node-dot::after{
    content:''; position:absolute; inset:-8px; border-radius:50%; opacity:0.35;
    animation:ping 2.4s ease-out infinite;
  }
  .node.ok .node-dot{background:#3FA796;} .node.ok .node-dot::after{background:#3FA796;}
  .node.watch .node-dot{background:var(--amber);} .node.watch .node-dot::after{background:var(--amber);}
  .node.critical .node-dot{background:var(--coral);} .node.critical .node-dot::after{background:var(--coral);}
  @keyframes ping{0%{transform:scale(0.6); opacity:0.5;} 100%{transform:scale(2.2); opacity:0;}}
  .node-label{
    position:absolute; top:16px; left:50%; transform:translateX(-50%); white-space:nowrap;
    font-size:0.65rem; font-family:'JetBrains Mono'; color:#EAF3F1; background:rgba(0,0,0,0.25);
    padding:2px 6px; border-radius:4px; opacity:0; transition:opacity .2s; pointer-events:none;
  }
  .node:hover .node-label{opacity:1;}
  .pulse-detail{
    position:relative; z-index:2; margin-top:14px; background:#12433F; border-radius:10px; padding:16px 18px;
    font-size:0.85rem; color:#E8E6DE; min-height:66px;
  }
  .pulse-detail b{color:#fff; font-family:'Fraunces';}
  .pulse-detail .tag{
    display:inline-block; font-family:'JetBrains Mono'; font-size:0.68rem; padding:2px 8px; border-radius:10px; margin-left:8px;
  }
  .tag.ok{background:rgba(63,167,150,0.2); color:#7FD8C6;}
  .tag.watch{background:rgba(201,122,30,0.25); color:#F2C879;}
  .tag.critical{background:rgba(192,57,43,0.25); color:#F2A99A;}

  /* STATS ROW */
  .stats-row{
    display:grid; grid-template-columns:repeat(4,1fr); gap:20px; margin-top:60px;
  }
  .stat-card{
    background:var(--panel); border:1px solid var(--line); border-radius:12px; padding:24px;
  }
  .stat-card .num{font-family:'Fraunces'; font-size:2rem; font-weight:600; color:var(--teal);}
  .stat-card .lbl{font-size:0.85rem; color:var(--muted); margin-top:4px;}

  /* FEATURES */
  .feat-grid{display:grid; grid-template-columns:repeat(3,1fr); gap:22px; margin-top:20px;}
  .feat-card{
    background:var(--panel); border:1px solid var(--line); border-radius:12px; padding:28px;
    transition:transform .2s, box-shadow .2s;
  }
  .feat-card:hover{transform:translateY(-4px); box-shadow:0 16px 32px rgba(18,48,44,0.08);}
  .feat-icon{
    width:44px; height:44px; border-radius:10px; background:var(--teal-tint); display:flex; align-items:center;
    justify-content:center; margin-bottom:16px;
  }
  .feat-icon svg{width:22px; height:22px; stroke:var(--teal);}
  .feat-card h3{font-family:'Fraunces'; font-size:1.1rem; font-weight:600; margin-bottom:8px; color:var(--ink);}
  .feat-card p{font-size:0.9rem; color:var(--muted); line-height:1.55;}

  /* ALERTS + FORECAST */
  .split{display:grid; grid-template-columns:1fr 1fr; gap:28px; align-items:start;}
  .panel{background:var(--panel); border:1px solid var(--line); border-radius:14px; padding:28px;}
  .panel h3{font-family:'Fraunces'; font-size:1.2rem; margin-bottom:18px; color:var(--ink);}
  .alert-item{
    display:flex; gap:12px; align-items:flex-start; padding:14px 0; border-bottom:1px solid var(--line);
  }
  .alert-item:last-child{border-bottom:none;}
  .alert-badge{
    font-family:'JetBrains Mono'; font-size:0.65rem; font-weight:600; padding:3px 8px; border-radius:5px; flex-shrink:0; margin-top:2px;
  }
  .alert-badge.critical{background:var(--coral-tint); color:var(--coral);}
  .alert-badge.watch{background:var(--amber-tint); color:var(--amber);}
  .alert-item .txt b{display:block; font-size:0.92rem; color:var(--ink); margin-bottom:2px;}
  .alert-item .txt span{font-size:0.8rem; color:var(--muted);}

  .forecast-controls{display:flex; gap:10px; margin-bottom:18px; flex-wrap:wrap;}
  .chip{
    font-family:'JetBrains Mono'; font-size:0.75rem; padding:7px 14px; border-radius:20px; border:1px solid var(--line);
    background:transparent; color:var(--muted); cursor:pointer; transition:all .2s;
  }
  .chip.active{background:var(--teal); border-color:var(--teal); color:#fff;}
  canvas#forecastChart{width:100%; height:200px;}
  .forecast-note{margin-top:14px; font-size:0.82rem; color:var(--muted); font-family:'JetBrains Mono';}
  .forecast-note b{color:var(--amber);}

  /* REDISTRIBUTION */
  .redis-card{
    margin-top:28px; background:var(--teal-tint); border-radius:14px; padding:26px 28px;
    display:flex; align-items:center; gap:22px; flex-wrap:wrap;
  }
  .redis-flow{display:flex; align-items:center; gap:14px; font-family:'JetBrains Mono'; font-size:0.85rem;}
  .redis-node{background:#fff; border-radius:8px; padding:10px 16px; border:1px solid var(--line);}
  .redis-arrow{color:var(--amber); font-size:1.3rem;}
  .redis-text{flex:1; min-width:240px; font-size:0.9rem; color:var(--ink);}
  .redis-text b{color:var(--teal);}

  /* FOOTER */
  footer{
    padding:50px 6% 34px; border-top:1px solid var(--line); display:flex; justify-content:space-between;
    align-items:center; flex-wrap:wrap; gap:16px;
  }
  footer p{color:var(--muted); font-size:0.85rem;}
  .foot-links{display:flex; gap:22px;}
  .foot-links a{color:var(--muted); font-size:0.85rem; text-decoration:none;}
  .foot-links a:hover{color:var(--teal);}

  @media (max-width:960px){
    .hero{grid-template-columns:1fr;}
    .stats-row{grid-template-columns:repeat(2,1fr);}
    .feat-grid{grid-template-columns:1fr;}
    .split{grid-template-columns:1fr;}
    nav ul{display:none;}
  }
  @media (prefers-reduced-motion:reduce){
    *{animation-duration:.001ms !important; transition-duration:.001ms !important;}
  }
</style>
</head>
<body>

<nav>
  <div class="logo"><span class="logo-mark"><svg viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="2"><path d="M12 2 3 7v6c0 5 4 8 9 9 5-1 9-4 9-9V7l-9-5Z"/></svg></span>ArogyaGrid</div>
  <ul>
    <li><a href="#pulse">District Pulse</a></li>
    <li><a href="#features">Features</a></li>
    <li><a href="#alerts">Alerts</a></li>
    <li><a href="#forecast">Forecast</a></li>
  </ul>
  <a href="#pulse" class="nav-cta">View Live Demo</a>
</nav>

<!-- HERO -->
<header class="hero" id="top">
  <div>
    <div class="eyebrow">District Health Intelligence</div>
    <h1>Every PHC & CHC, <br><em>one live pulse.</em></h1>
    <p>ArogyaGrid gives district health officers real-time visibility into stock, beds, footfall, and doctor attendance across every health centre — with AI that flags problems before they become shortages.</p>
    <div class="btn-row">
      <a href="#pulse" class="btn btn-primary">See the district live</a>
      <a href="#features" class="btn btn-ghost">Explore features</a>
    </div>
  </div>

  <div class="pulse-card" id="pulse">
    <div class="pulse-head">
      <span>DISTRICT PULSE — RAEBARELI</span>
      <div class="pulse-legend">
        <div><i style="background:#3FA796"></i>Healthy</div>
        <div><i style="background:#C97A1E"></i>Watch</div>
        <div><i style="background:#C0392B"></i>Critical</div>
      </div>
    </div>
    <div id="districtMap"></div>
    <div class="pulse-detail" id="pulseDetail">
      <b>Tap a centre</b> to see its live stock, bed, and doctor-attendance status.
    </div>
  </div>
</header>

<!-- STATS -->
<section style="padding-top:0;">
  <div class="stats-row">
    <div class="stat-card"><div class="num">42</div><div class="lbl">PHCs & CHCs monitored</div></div>
    <div class="stat-card"><div class="num">6</div><div class="lbl">Centres flagged this week</div></div>
    <div class="stat-card"><div class="num">83%</div><div class="lbl">Avg. doctor attendance</div></div>
    <div class="stat-card"><div class="num">2.1 hrs</div><div class="lbl">Avg. time to flag a shortage</div></div>
  </div>
</section>

<!-- FEATURES -->
<section id="features" style="background:var(--panel); border-top:1px solid var(--line); border-bottom:1px solid var(--line);">
  <div class="eyebrow">What It Tracks</div>
  <h2>Five signals, one dashboard</h2>
  <div class="feat-grid">
    <div class="feat-card">
      <div class="feat-icon"><svg viewBox="0 0 24 24" fill="none" stroke-linecap="round" stroke-linejoin="round"><rect x="4" y="8" width="16" height="12" rx="2"/><path d="M8 8V6a4 4 0 0 1 8 0v2"/></svg></div>
      <h3>Stock Monitoring</h3>
      <p>Live inventory for every medicine, with early stock-out warnings before shelves go empty.</p>
    </div>
    <div class="feat-card">
      <div class="feat-icon"><svg viewBox="0 0 24 24" fill="none" stroke-linecap="round" stroke-linejoin="round"><circle cx="9" cy="8" r="3"/><circle cx="17" cy="9" r="2.5"/><path d="M3 20c0-3 2.5-5 6-5s6 2 6 5"/><path d="M14 15c2.8 0 5 1.6 5 4.5"/></svg></div>
      <h3>Patient Footfall</h3>
      <p>Real-time patient counts and trend patterns, so staffing matches actual demand.</p>
    </div>
    <div class="feat-card">
      <div class="feat-icon"><svg viewBox="0 0 24 24" fill="none" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="11" width="18" height="7" rx="1.5"/><path d="M5 11V8a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v3"/><path d="M3 18v2M21 18v2"/></svg></div>
      <h3>Bed Availability</h3>
      <p>Live occupancy across every ward, visible district-wide — no more turned-away admissions.</p>
    </div>
    <div class="feat-card">
      <div class="feat-icon"><svg viewBox="0 0 24 24" fill="none" stroke-linecap="round" stroke-linejoin="round"><path d="M9 3h6M10 3v5L5 19a2 2 0 0 0 2 3h10a2 2 0 0 0 2-3l-5-11V3"/></svg></div>
      <h3>Doctor Attendance</h3>
      <p>Check-in based tracking with instant absence alerts, so patients aren't turned away.</p>
    </div>
    <div class="feat-card">
      <div class="feat-icon"><svg viewBox="0 0 24 24" fill="none" stroke-linecap="round" stroke-linejoin="round"><path d="M9 3H4v5M4 3l6 6"/><path d="M15 21h5v-5M20 21l-6-6"/><path d="M9 21H4v-5M4 21l6-6"/><path d="M15 3h5v5M20 3l-6 6"/></svg></div>
      <h3>Test Availability Audits</h3>
      <p>Tracks which diagnostic tests are actually available on-site, not just on paper.</p>
    </div>
    <div class="feat-card">
      <div class="feat-icon"><svg viewBox="0 0 24 24" fill="none" stroke-linecap="round" stroke-linejoin="round"><path d="M4 5h11M4 5a2 2 0 1 0 0 0M9 5v14M4 12h7M14 19l3-14 3 14M15.5 15h3"/></svg></div>
      <h3>Multilingual Interface</h3>
      <p>Voice and chat support in regional languages, built for the health worker who uses it daily.</p>
    </div>
  </div>
</section>

<!-- ALERTS + FORECAST -->
<section>
  <div class="eyebrow">Live Intelligence</div>
  <h2>Alerts today, forecast for tomorrow</h2>
  <div class="split" id="alerts">
    <div class="panel">
      <h3>Flagged for district admin action</h3>
      <div id="alertList"></div>
    </div>
    <div class="panel" id="forecast">
      <h3>7-day demand forecast</h3>
      <div class="forecast-controls">
        <button class="chip active" data-med="Oral Rehydration Salts">ORS</button>
        <button class="chip" data-med="Paracetamol">Paracetamol</button>
        <button class="chip" data-med="Iron Folic Acid">Iron Folic Acid</button>
      </div>
      <canvas id="forecastChart" width="600" height="200"></canvas>
      <div class="forecast-note">→ <b id="forecastMsg">Demand rising 18% this week</b> — reorder recommended within 3 days.</div>
    </div>
  </div>

  <div class="redis-card">
    <div class="redis-flow">
      <div class="redis-node">Sitapur CHC<br><span style="color:var(--muted); font-size:0.7rem;">Surplus: ORS</span></div>
      <div class="redis-arrow">→</div>
      <div class="redis-node">Maharajganj PHC<br><span style="color:var(--coral); font-size:0.7rem;">Critical low</span></div>
    </div>
    <div class="redis-text">
      <b>Smart redistribution:</b> move 400 units of ORS from Sitapur CHC to Maharajganj PHC — covers the projected shortfall through next week.
    </div>
    <button class="btn btn-primary" style="padding:10px 20px; font-size:0.85rem;">Approve transfer</button>
  </div>
</section>

<footer>
  <p>© 2026 ArogyaGrid — AI for district health centre management.</p>
  <div class="foot-links">
    <a href="#pulse">District Pulse</a>
    <a href="#features">Features</a>
    <a href="#forecast">Forecast</a>
  </div>
</footer>

<script>
  // ---- District Pulse Map ----
  const centres = [
    { name: "Raebareli CHC", status: "ok", x: 30, y: 40, stock: "92%", beds: "12/20 free", doctor: "Present" },
    { name: "Sitapur CHC", status: "ok", x: 55, y: 20, stock: "88%", beds: "8/15 free", doctor: "Present" },
    { name: "Maharajganj PHC", status: "critical", x: 72, y: 55, stock: "14%", beds: "1/10 free", doctor: "Present" },
    { name: "Bachhrawan PHC", status: "watch", x: 45, y: 68, stock: "46%", beds: "4/10 free", doctor: "Absent today" },
    { name: "Dalmau PHC", status: "ok", x: 18, y: 75, stock: "77%", beds: "6/12 free", doctor: "Present" },
    { name: "Lalganj CHC", status: "watch", x: 85, y: 30, stock: "39%", beds: "3/14 free", doctor: "Present" },
    { name: "Unchahar PHC", status: "ok", x: 60, y: 82, stock: "81%", beds: "5/10 free", doctor: "Present" }
  ];

  const mapEl = document.getElementById('districtMap');
  const detailEl = document.getElementById('pulseDetail');
  const statusLabel = { ok: "Healthy", watch: "Watch", critical: "Critical" };

  centres.forEach((c) => {
    const node = document.createElement('div');
    node.className = `node ${c.status}`;
    node.style.left = c.x + '%';
    node.style.top = c.y + '%';
    node.innerHTML = `<div class="node-dot"></div><div class="node-label">${c.name}</div>`;
    node.addEventListener('click', () => {
      detailEl.innerHTML = `<b>${c.name}</b> <span class="tag ${c.status}">${statusLabel[c.status]}</span><br>
        Stock health: ${c.stock} &nbsp;·&nbsp; Beds free: ${c.beds} &nbsp;·&nbsp; Doctor: ${c.doctor}`;
    });
    mapEl.appendChild(node);
  });

  // ---- Alerts feed ----
  const alerts = [
    { level: "critical", title: "Maharajganj PHC — ORS stock at 14%", meta: "Projected stock-out in 2 days · flagged to district admin" },
    { level: "watch", title: "Bachhrawan PHC — Doctor absent", meta: "No check-in logged for OPD shift · 3rd occurrence this month" },
    { level: "watch", title: "Lalganj CHC — Bed occupancy 79%", meta: "Trending toward capacity based on 7-day footfall" },
    { level: "critical", title: "Maharajganj PHC — Only 1 bed free", meta: "Redistribution recommended from Sitapur CHC" },
    { level: "watch", title: "Unchahar PHC — HB test kits low", meta: "Test availability audit flagged missing reagents" }
  ];
  const alertList = document.getElementById('alertList');
  alerts.forEach(a => {
    const div = document.createElement('div');
    div.className = 'alert-item';
    div.innerHTML = `<span class="alert-badge ${a.level}">${a.level.toUpperCase()}</span>
      <div class="txt"><b>${a.title}</b><span>${a.meta}</span></div>`;
    alertList.appendChild(div);
  });

  // ---- Forecast chart ----
  const forecastData = {
    "Oral Rehydration Salts": { hist: [40,44,42,50,55,60,68], msg: "Demand rising 18% this week" },
    "Paracetamol": { hist: [70,68,72,65,60,58,55], msg: "Demand easing 6% this week" },
    "Iron Folic Acid": { hist: [30,31,29,33,35,34,38], msg: "Demand steady, slight upward drift" }
  };
  const canvas = document.getElementById('forecastChart');
  const ctx = canvas.getContext('2d');

  function drawChart(data) {
    const w = canvas.width, h = canvas.height, pad = 30;
    ctx.clearRect(0,0,w,h);
    const max = Math.max(...data) * 1.15, min = 0;
    // grid
    ctx.strokeStyle = '#E5E5E0'; ctx.lineWidth = 1;
    for(let i=0;i<=3;i++){
      const y = pad + (h-2*pad) * i/3;
      ctx.beginPath(); ctx.moveTo(pad,y); ctx.lineTo(w-pad,y); ctx.stroke();
    }
    // line
    ctx.beginPath();
    ctx.strokeStyle = '#0F5C56'; ctx.lineWidth = 3; ctx.lineJoin='round';
    data.forEach((v,i) => {
      const x = pad + (w-2*pad) * i/(data.length-1);
      const y = h-pad - (h-2*pad) * (v-min)/(max-min);
      if(i===0) ctx.moveTo(x,y); else ctx.lineTo(x,y);
    });
    ctx.stroke();
    // fill
    ctx.lineTo(w-pad, h-pad); ctx.lineTo(pad, h-pad); ctx.closePath();
    ctx.fillStyle = 'rgba(15,92,86,0.08)'; ctx.fill();
    // points
    data.forEach((v,i) => {
      const x = pad + (w-2*pad) * i/(data.length-1);
      const y = h-pad - (h-2*pad) * (v-min)/(max-min);
      ctx.beginPath(); ctx.arc(x,y,4,0,Math.PI*2);
      ctx.fillStyle = '#C97A1E'; ctx.fill();
    });
  }

  function setMed(med) {
    drawChart(forecastData[med].hist);
    document.getElementById('forecastMsg').textContent = forecastData[med].msg;
  }
  document.querySelectorAll('#forecast .chip').forEach(chip => {
    chip.addEventListener('click', () => {
      document.querySelectorAll('#forecast .chip').forEach(c => c.classList.remove('active'));
      chip.classList.add('active');
      setMed(chip.dataset.med);
    });
  });
  setMed('Oral Rehydration Salts');
</script>

</body>
</html>
