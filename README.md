<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sanju Nivasini | FP&amp;A Associate Candidate Dashboard</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
  :root{
    --bg:#050816;
    --panel:rgba(10,17,31,0.9);
    --panel2:rgba(15,23,39,0.95);
    --surface:rgba(18,28,46,0.96);
    --border:rgba(125,144,179,0.18);
    --border-strong:rgba(125,144,179,0.32);
    --blue:#5d7cff;
    --blue-soft:#9eb2ff;
    --teal:#33c5b4;
    --rose:#f08e7b;
    --gold:#d8a96a;
    --green:#55c78b;
    --text:#f3f7ff;
    --sub:#9aa9c7;
    --muted:#7485a3;
    --shadow:0 22px 55px rgba(2, 8, 23, 0.38);
  }

  *{box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  body{
    margin:0;
    min-height:100vh;
    position:relative;
    overflow-x:hidden;
    background:
      radial-gradient(circle at 14% 0%, rgba(93,124,255,0.18), transparent 28%),
      radial-gradient(circle at 90% 0%, rgba(51,197,180,0.14), transparent 24%),
      radial-gradient(circle at 50% 100%, rgba(216,169,106,0.1), transparent 28%),
      linear-gradient(135deg, #040816 0%, #0b1427 55%, #050816 100%);
    color:var(--text);
    font-family:Inter, 'Segoe UI', system-ui, -apple-system, BlinkMacSystemFont, 'Roboto', Arial, sans-serif;
    line-height:1.5;
  }
  body::before{
    content:'';position:fixed;inset:0;
    background:radial-gradient(circle at center, transparent 0 20%, rgba(2,7,16,.32) 60%, rgba(2,7,16,.75) 100%);
    pointer-events:none;z-index:0;
  }
  body::after{
    content:'';position:fixed;inset:0;
    background-image:
      linear-gradient(rgba(255,255,255,.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(255,255,255,.03) 1px, transparent 1px);
    background-size:56px 56px;
    mask-image:linear-gradient(180deg, rgba(255,255,255,.2), transparent 85%);
    pointer-events:none;z-index:0;opacity:.35;
    animation:panGrid 22s linear infinite;
  }
  @keyframes panGrid{from{transform:translateY(0);}to{transform:translateY(40px);} }
  a{color:inherit;}
  ::selection{background:rgba(93,124,255,0.28);}

  .app-shell{position:relative;z-index:2;}
  .loading-screen{
    position:fixed;inset:0;z-index:9999;display:grid;place-items:center;background:radial-gradient(circle at center, rgba(8,14,24,.92), rgba(3,7,16,.98));
    transition:opacity .8s ease, visibility .8s ease;
  }
  .loading-screen.hidden{opacity:0;visibility:hidden;}
  .loading-core{display:grid;place-items:center;gap:16px;min-width:260px;text-align:center;}
  .loading-badge{
    width:70px;height:70px;border-radius:20px;display:grid;place-items:center;
    background:linear-gradient(135deg, rgba(93,124,255,.95), rgba(51,197,180,.85));
    color:#03111f;font-size:24px;font-weight:800;box-shadow:0 22px 45px rgba(93,124,255,.25);
    animation:floatBadge 1.8s ease-in-out infinite;
  }
  @keyframes floatBadge{0%,100%{transform:translateY(0) scale(1);}50%{transform:translateY(-8px) scale(1.02);} }
  .loading-copy{font-size:13px;letter-spacing:.16em;text-transform:uppercase;color:var(--sub);font-weight:700;}
  .loading-bar{width:220px;height:6px;border-radius:999px;background:rgba(255,255,255,.08);overflow:hidden;position:relative;}
  .loading-bar span{display:block;height:100%;width:38%;background:linear-gradient(90deg,var(--blue),var(--teal));border-radius:999px;animation:loadPulse 1.25s ease-in-out infinite;}
  @keyframes loadPulse{0%{transform:translateX(-110%);}100%{transform:translateX(310%);} }

  .scroll-progress{position:fixed;top:0;left:0;height:2px;width:0;background:linear-gradient(90deg,var(--blue),var(--teal));z-index:999;box-shadow:0 0 18px rgba(93,124,255,.34);}
  .cursor-glow{position:fixed;width:320px;height:320px;border-radius:50%;background:radial-gradient(circle, rgba(93,124,255,.16), transparent 65%);filter:blur(30px);pointer-events:none;transform:translate(-50%, -50%);opacity:0;z-index:1;transition:opacity .2s ease;}

  .particle-layer{position:fixed;inset:0;pointer-events:none;z-index:1;overflow:hidden;}
  .particle{position:absolute;display:block;border-radius:50%;background:rgba(255,255,255,.8);opacity:.5;animation:drift linear infinite;}
  @keyframes drift{0%{transform:translate3d(0,0,0) scale(1);opacity:0;}20%{opacity:.7;}100%{transform:translate3d(0,40px,0) scale(.2);opacity:0;}}

  header{
    position:sticky;top:0;z-index:50;
    background:rgba(4,10,20,0.74);
    border-bottom:1px solid rgba(130,154,190,0.2);
    backdrop-filter:blur(20px) saturate(1.1);
    box-shadow:0 10px 30px rgba(2,5,14,0.2);
  }
  .header-inner{max-width:1260px;margin:0 auto;padding:18px 24px 12px;}
  .brand-row{display:flex;align-items:center;justify-content:space-between;gap:14px;padding-bottom:14px;}
  .brand{display:flex;gap:12px;align-items:center;}
  .avatar{
    width:44px;height:44px;border-radius:14px;
    background:linear-gradient(135deg,var(--blue),var(--teal));
    display:flex;align-items:center;justify-content:center;
    font-weight:700;color:#03101f;font-size:15px;letter-spacing:.5px;
    flex:none;box-shadow:0 8px 20px rgba(93,124,255,0.22);
  }
  .brand-title{font-size:15px;font-weight:700;line-height:1.2;}
  .brand-sub{font-size:11.5px;color:var(--muted);letter-spacing:.04em;}
  .brand-chip{display:inline-flex;align-items:center;gap:6px;font-size:10.5px;font-weight:700;letter-spacing:.06em;color:#03101f;background:linear-gradient(135deg,var(--gold),#f8d281);border-radius:999px;padding:4px 10px;margin-top:5px;}

  .utility-bar{
    display:flex;justify-content:space-between;align-items:center;gap:12px;
    padding-top:12px;border-top:1px solid rgba(255,255,255,0.06);
  }
  .breadcrumbs{display:flex;align-items:center;gap:8px;font-size:12px;color:var(--muted);letter-spacing:.08em;text-transform:uppercase;}
  .breadcrumbs .slash{opacity:.5;}
  .search-pill{display:flex;align-items:center;gap:8px;padding:8px 12px;border:1px solid var(--border);border-radius:999px;background:rgba(255,255,255,0.04);min-width:260px;}
  .search-pill input{width:100%;background:transparent;border:none;outline:none;color:var(--text);font-size:13px;font-family:inherit;}
  .search-pill input::placeholder{color:var(--muted);}
  .status-pill{display:inline-flex;align-items:center;gap:6px;padding:7px 10px;border-radius:999px;background:rgba(51,197,180,0.12);color:var(--teal);font-size:11px;font-weight:700;letter-spacing:.06em;text-transform:uppercase;border:1px solid rgba(51,197,180,0.2);}

  nav{display:flex;flex-wrap:wrap;gap:7px;padding-bottom:8px;}
  nav button{
    background:transparent;border:1px solid transparent;color:var(--sub);
    font-size:12.3px;padding:8px 12px;border-radius:999px;cursor:pointer;
    font-family:inherit;transition:all 180ms ease;white-space:nowrap;letter-spacing:.06em;text-transform:uppercase;font-weight:700;position:relative;
  }
  nav button:hover{color:var(--text);background:rgba(255,255,255,0.04);border-color:rgba(93,124,255,.18);transform:translateY(-1px);}
  nav button.active{color:#fff;background:linear-gradient(135deg, rgba(93,124,255,.18), rgba(51,197,180,.12));border-color:rgba(93,124,255,.28);box-shadow:inset 0 1px 0 rgba(255,255,255,.05);}

  main{max-width:1260px;margin:0 auto;padding:30px 24px 90px;}
  section{display:none;animation:fade .45s cubic-bezier(.22,.61,.36,1);}
  section.active{display:block;}
  @keyframes fade{from{opacity:0;transform:translateY(8px);}to{opacity:1;transform:translateY(0);}}

  .eyebrow{font-size:11.5px;letter-spacing:.16em;text-transform:uppercase;color:var(--teal);font-weight:700;margin-bottom:6px;}
  h2.section-title{font-size:28px;margin:0 0 8px;font-weight:800;letter-spacing:-.015em;}
  p.section-sub{color:var(--sub);font-size:13.5px;margin:0 0 24px;max-width:74ch;line-height:1.6;}

  .overview-grid{display:grid;grid-template-columns:1.25fr .95fr;gap:24px;margin-bottom:28px;}
  @media(max-width:900px){.overview-grid{grid-template-columns:1fr;}}
  .hero{
    background:linear-gradient(135deg, rgba(93,124,255,0.12), rgba(255,255,255,0.02));
    border:1px solid rgba(93,124,255,.22);
    border-radius:18px;padding:26px;box-shadow:var(--shadow);
    position:relative;overflow:hidden;
  }
  .hero::before{content:'';position:absolute;inset:-1px;background:linear-gradient(120deg, transparent, rgba(255,255,255,.08), transparent);transform:translateX(-100%);transition:transform .75s ease;pointer-events:none;}
  .hero:hover::before{transform:translateX(100%);} 
  .hero h1{font-size:38px;line-height:1.08;font-weight:800;letter-spacing:-.015em;margin:0 0 16px;}
  .hero p.lead{color:var(--sub);font-size:15px;line-height:1.65;margin:0 0 20px;max-width:56ch;}
  .stat-mini-grid{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:12px;margin-bottom:18px;}
  @media(max-width:640px){.stat-mini-grid{grid-template-columns:repeat(2,1fr);}}
  .stat-mini{
    background:rgba(255,255,255,0.03);border:1px solid var(--border);border-radius:12px;padding:14px 14px 12px;
    transition:transform .2s ease,border-color .2s ease, box-shadow .2s ease;
    min-height:118px;position:relative;overflow:hidden;
  }
  .stat-mini::after{content:'';position:absolute;inset:0;background:linear-gradient(140deg, rgba(255,255,255,.08), transparent 40%);pointer-events:none;}
  .stat-mini:hover{transform:translateY(-2px);border-color:var(--border-strong);box-shadow:0 12px 24px rgba(2,8,23,0.18);}
  .stat-mini .icon{width:28px;height:28px;border-radius:10px;display:grid;place-items:center;background:rgba(93,124,255,.12);font-size:13px;margin-bottom:9px;color:var(--blue-soft);}
  .stat-mini .label{font-size:10.5px;text-transform:uppercase;letter-spacing:.12em;color:var(--muted);font-weight:700;margin-bottom:6px;}
  .stat-mini .value{font-size:21px;font-weight:800;font-variant-numeric:tabular-nums;}
  .trend{margin-top:7px;font-size:11px;font-weight:700;display:inline-flex;align-items:center;gap:5px;color:var(--green);} 
  .trend.warn{color:var(--gold);} 
  .cta-row{display:flex;flex-wrap:wrap;gap:10px;}
  .btn{
    border-radius:999px;padding:11px 16px;font-size:13.5px;font-weight:700;cursor:pointer;
    border:1px solid var(--border);background:rgba(255,255,255,0.04);color:var(--text);
    font-family:inherit;transition:all .18s ease;letter-spacing:.01em;
  }
  .btn.primary{background:linear-gradient(135deg,var(--blue),#3f67d9);border-color:transparent;color:#fff;box-shadow:0 8px 18px rgba(93,124,255,0.22);}
  .btn.secondary{background:rgba(255,255,255,0.05);}
  .btn:hover{transform:translateY(-1px);border-color:var(--blue-soft);}
  .btn:focus-visible{outline:2px solid rgba(93,124,255,.45);outline-offset:2px;}

  .side-panel{background:var(--panel);border:1px solid var(--border);border-radius:16px;padding:22px;box-shadow:var(--shadow);position:relative;overflow:hidden;}
  .side-panel::before{content:'';position:absolute;inset:0;background:linear-gradient(140deg, rgba(255,255,255,.05), transparent 35%);pointer-events:none;}
  .side-panel h3{font-size:19px;margin:0 0 10px;font-weight:800;line-height:1.3;}
  .side-panel p{color:var(--sub);font-size:13.5px;line-height:1.6;margin:0 0 18px;}
  .side-stat-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px;}
  .side-stat{background:rgba(255,255,255,0.03);border:1px solid var(--border);border-radius:10px;padding:12px 14px;}
  .side-stat .label{font-size:10px;text-transform:uppercase;letter-spacing:.08em;color:var(--muted);font-weight:700;margin-bottom:6px;}
  .side-stat .value{font-size:18px;font-weight:800;color:var(--blue-soft);font-variant-numeric:tabular-nums;}

  .panel{background:var(--panel);border:1px solid var(--border);border-radius:14px;padding:20px;box-shadow:var(--shadow);transition:border-color .18s ease, transform .18s ease, box-shadow .18s ease;position:relative;overflow:hidden;}
  .panel::before{content:'';position:absolute;inset:0;background:linear-gradient(120deg, transparent, rgba(255,255,255,.05), transparent);transform:translateX(-100%);transition:transform .75s ease;pointer-events:none;}
  .panel:hover{border-color:var(--border-strong);transform:translateY(-2px);box-shadow:0 14px 30px rgba(2,8,23,.24);}
  .panel:hover::before{transform:translateX(100%);} 
  .panel h4{margin:0 0 14px;font-size:15px;font-weight:700;}
  .two-col{display:grid;grid-template-columns:1fr 1fr;gap:20px;margin-bottom:20px;}
  @media(max-width:860px){.two-col{grid-template-columns:1fr;}}
  .chart-wrap{position:relative;height:260px;padding-top:4px;}
  .hint{font-size:11px;color:var(--muted);margin-top:8px;}

  table{width:100%;border-collapse:collapse;font-size:13px;}
  th{text-align:left;color:var(--muted);font-weight:700;text-transform:uppercase;font-size:10.5px;letter-spacing:.12em;padding:8px 10px;border-bottom:1px solid rgba(255,255,255,0.08);}
  td{padding:10px 10px;border-bottom:1px solid rgba(255,255,255,0.08);color:var(--text);vertical-align:top;}
  th.numeric, td.numeric{text-align:right;font-variant-numeric:tabular-nums;}
  tr:last-child td{border-bottom:none;}
  td.acct{font-weight:700;color:var(--blue-soft);}
  tbody tr:hover{background:rgba(255,255,255,.02);} 

  .tree{display:flex;flex-direction:column;gap:10px;}
  .tree-row{background:rgba(255,255,255,0.03);border:1px solid var(--border);border-radius:10px;padding:12px 16px;font-weight:700;font-size:14px;}
  .tree-row .sub{display:block;margin-top:8px;font-weight:400;font-size:12.5px;color:var(--sub);}
  .tree-row .sub span{display:inline-block;background:rgba(255,255,255,0.04);border:1px solid var(--border);border-radius:6px;padding:3px 9px;margin:2px 6px 2px 0;}

  .fit-badge{display:inline-flex;align-items:center;gap:5px;font-size:10.5px;font-weight:700;letter-spacing:.04em;text-transform:uppercase;color:#041122;background:var(--green);border-radius:999px;padding:4px 9px;white-space:nowrap;}
  .fit-badge.pref{background:var(--gold);}
  td.req{font-weight:600;color:var(--text);max-width:260px;}
  td.evi{color:var(--sub);}

  .scenario-grid{display:grid;grid-template-columns:1fr 1fr;gap:22px;}
  @media(max-width:860px){.scenario-grid{grid-template-columns:1fr;}}
  .slider-row{margin-bottom:22px;}
  .slider-row label{display:flex;justify-content:space-between;font-size:13px;font-weight:600;margin-bottom:8px;color:var(--sub);}
  .slider-row output{color:var(--text);font-weight:800;}
  input[type=range]{width:100%;accent-color:var(--blue);}
  .output-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px;}
  @media(max-width:640px){.output-grid{grid-template-columns:1fr 1fr;}}
  .out-card{background:rgba(255,255,255,0.03);border:1px solid var(--border);border-radius:10px;padding:12px 14px;}
  .out-card .label{font-size:10px;text-transform:uppercase;color:var(--muted);font-weight:700;letter-spacing:.08em;margin-bottom:6px;}
  .out-card .value{font-size:18px;font-weight:800;color:var(--teal);font-variant-numeric:tabular-nums;}
  .signal-box{margin-top:20px;background:rgba(93,124,255,.08);border:1px solid rgba(93,124,255,.24);border-radius:10px;padding:14px 16px;font-size:13.5px;color:var(--blue-soft);line-height:1.5;}

  .story-card{background:var(--panel);border:1px solid var(--border);border-radius:14px;padding:22px;margin-bottom:18px;box-shadow:var(--shadow);position:relative;overflow:hidden;}
  .story-card::before{content:'';position:absolute;inset:0;background:linear-gradient(120deg, transparent, rgba(255,255,255,.05), transparent);transform:translateX(-100%);transition:transform .75s ease;pointer-events:none;}
  .story-card:hover::before{transform:translateX(100%);} 
  .story-card h4{font-size:17px;margin:0 0 10px;font-weight:800;}
  .story-card ul{margin:10px 0 0;padding-left:18px;color:var(--sub);font-size:13.5px;line-height:1.7;}
  .story-tag{display:inline-block;font-size:10.5px;font-weight:700;letter-spacing:.08em;text-transform:uppercase;color:var(--teal);background:rgba(51,197,180,.1);border:1px solid rgba(51,197,180,.24);border-radius:999px;padding:4px 9px;margin-bottom:10px;}

  .skill-grid{display:grid;grid-template-columns:1fr 1fr;gap:16px;}
  @media(max-width:640px){.skill-grid{grid-template-columns:1fr;}}
  .skill{background:var(--panel);border:1px solid var(--border);border-radius:10px;padding:16px 18px;}
  .skill-top{display:flex;justify-content:space-between;font-size:14px;font-weight:700;margin-bottom:10px;}
  .bar-track{height:7px;border-radius:999px;background:rgba(255,255,255,0.05);overflow:hidden;}
  .bar-fill{height:100%;border-radius:999px;background:linear-gradient(90deg,var(--blue),var(--gold));}

  .toolkit-grid{display:grid;grid-template-columns:1fr 1fr;gap:16px;}
  @media(max-width:760px){.toolkit-grid{grid-template-columns:1fr;}}
  .fw-card{background:var(--panel);border:1px solid var(--border);border-radius:10px;padding:18px 20px;}
  .fw-card h4{margin:0 0 10px;font-size:15px;font-weight:800;color:var(--blue-soft);}
  .fw-card p{margin:0 0 6px;font-size:12.5px;color:var(--sub);line-height:1.55;}
  .fw-card b{color:var(--text);}

  footer{text-align:center;color:var(--muted);font-size:12px;padding:36px 24px 50px;border-top:1px solid rgba(255,255,255,0.06);margin-top:20px;}
  .table-scroll{overflow-x:auto;}

  .reveal{opacity:0;transform:translateY(24px) scale(.985);transition:opacity .7s cubic-bezier(.22,.61,.36,1), transform .7s cubic-bezier(.22,.61,.36,1);}
  .reveal.in-view{opacity:1;transform:none;}
</style>
</head>
<body>
  <div class="loading-screen" id="loadingScreen">
    <div class="loading-core">
      <div class="loading-badge">SN</div>
      <div class="loading-copy">Initializing executive review</div>
      <div class="loading-bar"><span></span></div>
    </div>
  </div>
  <div class="scroll-progress" aria-hidden="true"></div>
  <div class="cursor-glow" id="cursorGlow"></div>
  <div class="particle-layer" id="particleLayer"></div>
  <div class="app-shell">

<header>
  <div class="header-inner">
    <div class="brand-row">
      <div class="brand">
        <div class="avatar">SN</div>
        <div>
          <div class="brand-title">Sanju Nivasini<br>FP&amp;A Dashboard</div>
          <div class="brand-chip">Financial Analysis (FP&amp;A)</div>
        </div>
      </div>
      <div class="cta-row" style="margin-top:2px;">
        <button class="btn primary" onclick="showQR()">Share / QR Code</button>
      </div>
    </div>
    <div class="utility-bar">
      <div class="breadcrumbs">Executive Review <span class="slash">/</span> FP&amp;A <span class="slash">/</span> Dashboard</div>
      <div class="cta-row">
        <label class="search-pill">
          <span>⌕</span>
          <input type="text" placeholder="Search metrics, stories, panels">
        </label>
        <span class="status-pill">● Live Executive Readout</span>
      </div>
    </div>
    <nav id="tabnav"></nav>
  </div>
</header>

<main>

  <!-- OVERVIEW -->
  <section id="overview" class="active">
    <div class="overview-grid">
      <div class="hero">
        <h1>A Trusted Partner for Business, Finance &amp; FP&amp;A Teams</h1>
        <p class="lead">FP&amp;A professional specializing in financial planning, forecasting and budgeting, variance analysis, ad hoc executive reporting, and process automation — built to support a broad range of FP&amp;A and financial analysis roles.</p>
        <div class="stat-mini-grid">
          <div class="stat-mini"><div class="icon"><i data-lucide="badge-dollar-sign"></i></div><div class="label">Revenue Governed</div><div class="value">$121.4M</div><div class="trend">▲ +8.4% vs plan</div></div>
          <div class="stat-mini"><div class="icon"><i data-lucide="bar-chart-3"></i></div><div class="label">Forecast/Budget Cycles Led</div><div class="value">48</div><div class="trend">▲ 12 cycle uplift</div></div>
          <div class="stat-mini"><div class="icon"><i data-lucide="clipboard-list"></i></div><div class="label">Senior Mgmt Reviews Supported</div><div class="value">140+</div><div class="trend warn">● Steady cadence</div></div>
          <div class="stat-mini"><div class="icon"><i data-lucide="file-text"></i></div><div class="label">Ad Hoc Reports Delivered</div><div class="value">52</div><div class="trend">▲ 2.1x faster turn</div></div>
          <div class="stat-mini"><div class="icon"><i data-lucide="trending-up"></i></div><div class="label">Dashboards Designed</div><div class="value">12</div><div class="trend">▲ 6 executive views</div></div>
          <div class="stat-mini"><div class="icon"><i data-lucide="zap"></i></div><div class="label">Automation Initiatives</div><div class="value">31</div><div class="trend">▲ 30% cycle gain</div></div>
        </div>
        <div class="cta-row">
          <button class="btn primary" onclick="go('fit')">Open Role Fit Scorecard</button>
          <button class="btn secondary" onclick="go('scenario')">Run Forecast Simulator</button>
          <button class="btn secondary" onclick="showQR()">Download Resume</button>
        </div>
        <div class="cta-row" style="margin-top:10px;">
          <button class="btn" onclick="go('stories')">View Job-Responsibility Impact Stories</button>
        </div>
      </div>
      <div class="side-panel">
        <h3>What happened. Why it happened. What to do next.</h3>
        <p>Every panel here maps to a specific line in the Associate, Financial Analysis job description — built for recruiter and hiring-manager review, not just as a static resume.</p>
        <div class="side-stat-grid">
          <div class="side-stat"><div class="label">Forecast Accuracy</div><div class="value">97.8%</div></div>
          <div class="side-stat"><div class="label">Relevant Experience</div><div class="value">6+ Yrs</div></div>
          <div class="side-stat"><div class="label">Advanced Excel/PPT</div><div class="value">Proficient</div></div>
          <div class="side-stat"><div class="label">Special Projects Delivered</div><div class="value">65+</div></div>
        </div>
      </div>
    </div>
  </section>

  <!-- ROLE FIT SCORECARD -->
  <section id="fit">
    <div class="eyebrow">Section 1</div>
    <h2 class="section-title">Role Fit Scorecard</h2>
    <p class="section-sub">Every required and preferred qualification from the Associate, Financial Analysis posting, matched line-by-line against demonstrated experience.</p>

    <div class="panel" style="margin-bottom:20px;">
      <h4>Required Qualifications, Capabilities &amp; Skills</h4>
      <div class="table-scroll">
      <table>
        <tr><th>JD Requirement</th><th>Evidence</th><th>Status</th></tr>
        <tr><td class="req">Bachelor's in Accounting, Finance, or technical subject</td><td class="evi">B.S./B.A. in Finance; 6+ years applying it in FP&amp;A and controller-adjacent roles</td><td><span class="fit-badge">Met</span></td></tr>
        <tr><td class="req">Advanced Excel &amp; PowerPoint; data mining and manipulation of large datasets</td><td class="evi">Built 26 scenario/valuation models and 12 executive dashboards from raw multi-source data</td><td><span class="fit-badge">Met</span></td></tr>
        <tr><td class="req">Inquisitive, enthusiastic, diligent, capable of challenging peers</td><td class="evi">Led margin-recovery challenge sessions that reshaped a $121M portfolio's cost strategy</td><td><span class="fit-badge">Met</span></td></tr>
        <tr><td class="req">Strong verbal &amp; written communication; articulate complex issues clearly</td><td class="evi">Authored variance commentary and narratives for 140+ senior management reviews</td><td><span class="fit-badge">Met</span></td></tr>
        <tr><td class="req">Ability to create ad hoc reporting for senior management</td><td class="evi">52 ad hoc reports delivered on same-day/next-day turnaround for leadership requests</td><td><span class="fit-badge">Met</span></td></tr>
        <tr><td class="req">Track record executing special projects with little lead time</td><td class="evi">65+ strategic recommendations delivered under compressed, ad hoc timelines</td><td><span class="fit-badge">Met</span></td></tr>
        <tr><td class="req">Thrives under pressure and tight deadlines</td><td class="evi">Owned monthly close and forecast cycles on fixed, non-negotiable reporting calendars</td><td><span class="fit-badge">Met</span></td></tr>
        <tr><td class="req">Trusted partner across Business, Finance, and FP&amp;A teams</td><td class="evi">Embedded business partner for six product/account teams across 48 planning cycles</td><td><span class="fit-badge">Met</span></td></tr>
        <tr><td class="req">Self-starter with strong time management/prioritization</td><td class="evi">Ran concurrent forecast, variance, and ad hoc workstreams without missed deadlines</td><td><span class="fit-badge">Met</span></td></tr>
        <tr><td class="req">Strong analytical/problem-solving; analyze large data, present conclusions concisely</td><td class="evi">Converted six-account, multi-year datasets into three-slide leadership decisions</td><td><span class="fit-badge">Met</span></td></tr>
      </table>
      </div>
    </div>

    <div class="panel">
      <h4>Preferred Qualifications, Capabilities &amp; Skills</h4>
      <div class="table-scroll">
      <table>
        <tr><th>JD Preference</th><th>Evidence</th><th>Status</th></tr>
        <tr><td class="req">5+ years, preferably Financial Services and/or accounting/controller background</td><td class="evi">6+ years in FP&amp;A with direct accounting reconciliation and controller-adjacent ownership</td><td><span class="fit-badge pref">Strong Match</span></td></tr>
      </table>
      </div>
    </div>
  </section>

  <!-- INTELLIGENCE LAB -->
  <section id="lab">
    <div class="eyebrow">Section 2</div>
    <h2 class="section-title">Financial Analysis &amp; Reporting Lab</h2>
    <p class="section-sub">Demonstrates the JD's "analysis of financial/business metrics" and "delivery of weekly/monthly/quarterly management reporting" responsibilities.</p>

    <div class="two-col">
      <div class="panel">
        <h4>Growth vs Margin Matrix</h4>
        <div class="chart-wrap"><canvas id="chartGrowthMargin"></canvas></div>
        <div class="hint">Hover bubbles for leadership recommendations.</div>
      </div>
      <div class="panel">
        <h4>Portfolio Performance Matrix</h4>
        <div class="chart-wrap"><canvas id="chartBCG"></canvas></div>
        <div class="hint">Hover quadrants and accounts for strategic actions.</div>
      </div>
    </div>

    <div class="two-col">
      <div class="panel">
        <h4>Revenue-Margin Heatmap</h4>
        <div class="table-scroll">
        <table>
          <tr><th>Account</th><th>Revenue</th><th>Growth</th><th>GM%</th><th>OM%</th><th>Forecast Acc.</th></tr>
          <tr><td class="acct">Alpha</td><td>$28.4M</td><td>14%</td><td>43%</td><td>24%</td><td>98%</td></tr>
          <tr><td class="acct">Delta</td><td>$21.1M</td><td>6%</td><td>42%</td><td>25%</td><td>98%</td></tr>
          <tr><td class="acct">Nova</td><td>$18.9M</td><td>16%</td><td>34%</td><td>13%</td><td>95%</td></tr>
          <tr><td class="acct">Sigma</td><td>$12.3M</td><td>3%</td><td>30%</td><td>18%</td><td>92%</td></tr>
          <tr><td class="acct">Vertex</td><td>$14.6M</td><td>11%</td><td>38%</td><td>18%</td><td>92%</td></tr>
          <tr><td class="acct">Horizon</td><td>$9.7M</td><td>2%</td><td>38%</td><td>13%</td><td>90%</td></tr>
        </table>
        </div>
      </div>
      <div class="panel">
        <h4>Variance Bridge (QoQ OM% vs Prior Forecast)</h4>
        <div class="chart-wrap"><canvas id="chartWaterfall"></canvas></div>
      </div>
    </div>

    <div class="panel">
      <h4>Financial Planning Value Driver Tree</h4>
      <div class="tree">
        <div class="tree-row">Revenue &amp; Demand Planning
          <span class="sub"><span>Volume</span><span>Rate</span><span>Consumption/Utilization</span></span>
        </div>
        <div class="tree-row">Budget vs Forecast Performance</div>
        <div class="tree-row">Operating Margin</div>
        <div class="tree-row">Senior Management Reporting</div>
      </div>
    </div>
  </section>

  <!-- FORECAST SIMULATOR -->
  <section id="scenario">
    <div class="eyebrow">Section 3</div>
    <h2 class="section-title">Budget &amp; Forecast Scenario Simulator</h2>
    <p class="section-sub">Mirrors the JD's "annual/continual financial planning activities" and "consumption/demand planning, budgeting" responsibilities.</p>
    <div class="scenario-grid">
      <div class="panel">
        <div class="slider-row">
          <label>Revenue Growth % <output id="outGrowth">10</output></label>
          <input type="range" id="inGrowth" min="0" max="30" value="10" oninput="updateScenario()">
        </div>
        <div class="slider-row">
          <label>Headcount Change % <output id="outHC">4</output></label>
          <input type="range" id="inHC" min="-10" max="20" value="4" oninput="updateScenario()">
        </div>
        <div class="slider-row">
          <label>Utilization % <output id="outUtil">80</output></label>
          <input type="range" id="inUtil" min="50" max="100" value="80" oninput="updateScenario()">
        </div>
      </div>
      <div class="panel">
        <div class="output-grid">
          <div class="out-card"><div class="label">Revenue</div><div class="value" id="valRevenue">$133.5M</div></div>
          <div class="out-card"><div class="label">GM%</div><div class="value" id="valGM">40.2%</div></div>
          <div class="out-card"><div class="label">OM%</div><div class="value" id="valOM">23.6%</div></div>
          <div class="out-card"><div class="label">RPP</div><div class="value" id="valRPP">$142,671</div></div>
          <div class="out-card"><div class="label">Operating Profit</div><div class="value" id="valOP">$31.5M</div></div>
          <div class="out-card"><div class="label">Forecast-vs-Budget Variance</div><div class="value" id="valCR">$2.9M</div></div>
        </div>
        <div class="signal-box" id="cfoSignal">FP&amp;A Action Signal: Current inputs support continued investment — margin trajectory remains within governed range.</div>
      </div>
    </div>
  </section>

  <!-- IMPACT STORIES -->
  <section id="stories">
    <div class="eyebrow">Section 4</div>
    <h2 class="section-title">Job-Responsibility Impact Stories</h2>
    <p class="section-sub">Each story is tied directly to a bullet in the Associate, Financial Analysis job description.</p>

    <div class="story-card">
      <div class="story-tag">Reporting &amp; Dashboards</div>
      <h4>Executive Reporting &amp; Dashboard Redesign</h4>
      <ul>
        <li>Replaced static month-end decks with live dashboards, addressing "helping design new reports and dashboards to efficiently deliver financial results to senior management."</li>
        <li>Cut executive review prep time while lifting forecast accuracy to 97.8%.</li>
        <li>Gave leadership a single source of truth spanning 48 forecast/budget cycles.</li>
      </ul>
    </div>
    <div class="story-card">
      <div class="story-tag">Variance Analysis</div>
      <h4>Variance-to-Forecast Commentary Program</h4>
      <ul>
        <li>Ran variance analysis to identify key result drivers, directly matching "performing variance analysis... presenting commentary to senior managers, explaining changes from prior forecasts/budgets."</li>
        <li>Standardized commentary templates that cut write-up time by 40%.</li>
        <li>Findings converted into a governed action plan reviewed each cycle.</li>
      </ul>
    </div>
    <div class="story-card">
      <div class="story-tag">Controls &amp; Automation</div>
      <h4>Process Streamlining &amp; Automation Rollout</h4>
      <ul>
        <li>Introduced automated applications and process improvements, matching "enhancing controls and streamlining processes, introducing automation where possible."</li>
        <li>Reduced reporting cycle time and reconciliation error rate through automated checks.</li>
        <li>Maintained and improved financial and reporting systems alongside the rollout.</li>
      </ul>
    </div>
    <div class="story-card">
      <div class="story-tag">Business Cases</div>
      <h4>Financial Business Case Development</h4>
      <ul>
        <li>Built financial business cases supporting business initiatives, per "creating financial business cases supporting business initiatives."</li>
        <li>Quantified ROI, payback, and risk-adjusted margin for leadership funding decisions.</li>
        <li>Supported 65+ strategic recommendations adopted into planning cycles.</li>
      </ul>
    </div>
  </section>

  <!-- DECISION PLAYBOOK -->
  <section id="playbook">
    <div class="eyebrow">Section 5</div>
    <h2 class="section-title">FP&amp;A Decision Playbook</h2>
    <p class="section-sub">Framework applied to each core job responsibility in the posting.</p>
    <div class="panel">
      <div class="table-scroll">
      <table>
        <tr><th>Job Responsibility (from JD)</th><th>Framework / Deliverable Used</th></tr>
        <tr><td>Managing annual/continual financial planning &amp; forecasting</td><td>Financial Planning &amp; Forecast Model</td></tr>
        <tr><td>Efficiency reporting, location strategy, span of control</td><td>Efficiency Reporting Framework</td></tr>
        <tr><td>Creating financial business cases supporting initiatives</td><td>Business Case Framework</td></tr>
        <tr><td>Reporting on/monitoring key metrics, data quality</td><td>KPI Governance Dashboard</td></tr>
        <tr><td>Variance analysis vs prior forecast/budget</td><td>Variance Bridge &amp; Commentary Model</td></tr>
        <tr><td>Reports/dashboards for senior management</td><td>Reporting &amp; Dashboard Design Framework</td></tr>
        <tr><td>Streamlining cycle times through automation</td><td>Automation &amp; Controls Framework</td></tr>
        <tr><td>Special projects with little lead time</td><td>Rapid Turnaround Playbook</td></tr>
      </table>
      </div>
    </div>
  </section>

  <!-- AUTOMATION & CONTROLS -->
  <section id="cost">
    <div class="eyebrow">Section 6</div>
    <h2 class="section-title">Process Automation &amp; Controls</h2>
    <p class="section-sub">Directly evidences "enhancing controls and streamlining processes, introducing automation where possible."</p>
    <div class="two-col">
      <div class="panel">
        <h4>Process &amp; Controls Tree</h4>
        <div class="tree">
          <div class="tree-row">Reporting Cycle Time
            <span class="sub"><span>Data Validation</span><span>Template Standardization</span><span>Automation</span></span>
          </div>
          <div class="tree-row">Controls Enhancement
            <span class="sub"><span>Reconciliation Accuracy</span><span>Exception Management</span><span>Audit Trail</span></span>
          </div>
          <div class="tree-row">Special Projects &amp; Ad Hoc Turnaround</div>
        </div>
      </div>
      <div class="panel">
        <h4>Initiatives Tracked</h4>
        <div class="table-scroll">
        <table>
          <tr><th>Initiative</th><th>Lever</th><th>Impact</th></tr>
          <tr><td>Automated Reporting Rollout</td><td>Cycle Time</td><td>-30% cycle time</td></tr>
          <tr><td>Reconciliation Automation</td><td>Controls</td><td>-45% error rate</td></tr>
          <tr><td>Ad Hoc Reporting Template Library</td><td>Turnaround</td><td>2 days &rarr; 4 hrs</td></tr>
          <tr><td>Dashboard Self-Service Rollout</td><td>Efficiency</td><td>12 dashboards deployed</td></tr>
        </table>
        </div>
        <div class="hint">Total automation initiatives delivered: 31</div>
      </div>
    </div>
  </section>

  <!-- CAPABILITY CENTER -->
  <section id="capability">
    <div class="eyebrow">Section 7</div>
    <h2 class="section-title">Capability Center</h2>
    <p class="section-sub">Skill weighting reflects the required qualifications listed in the posting.</p>
    <div class="skill-grid">
      <div class="skill"><div class="skill-top"><span>Advanced Excel &amp; PowerPoint</span><span>95%</span></div><div class="bar-track"><div class="bar-fill" style="width:95%"></div></div></div>
      <div class="skill"><div class="skill-top"><span>Ad Hoc Reporting &amp; Data Mining</span><span>93%</span></div><div class="bar-track"><div class="bar-fill" style="width:93%"></div></div></div>
      <div class="skill"><div class="skill-top"><span>Variance Analysis &amp; Commentary</span><span>92%</span></div><div class="bar-track"><div class="bar-fill" style="width:92%"></div></div></div>
      <div class="skill"><div class="skill-top"><span>Business Partnering (Business/Finance/FP&amp;A)</span><span>90%</span></div><div class="bar-track"><div class="bar-fill" style="width:90%"></div></div></div>
      <div class="skill"><div class="skill-top"><span>Special Projects Under Tight Deadlines</span><span>91%</span></div><div class="bar-track"><div class="bar-fill" style="width:91%"></div></div></div>
      <div class="skill"><div class="skill-top"><span>Analytical &amp; Problem Solving</span><span>94%</span></div><div class="bar-track"><div class="bar-fill" style="width:94%"></div></div></div>
    </div>
  </section>

  <!-- EXECUTIVE TOOLKIT -->
  <section id="toolkit">
    <div class="eyebrow">Section 8</div>
    <h2 class="section-title">FP&amp;A Toolkit</h2>
    <p class="section-sub">The frameworks behind every job responsibility in the posting.</p>
    <div class="toolkit-grid">
      <div class="fw-card">
        <h4>Financial Planning &amp; Forecasting Framework</h4>
        <p><b>Purpose:</b> Manage annual/continual planning and month-end financial performance.</p>
        <p><b>Metrics Used:</b> Forecast Accuracy, Budget Variance, Demand Planning.</p>
        <p><b>Leadership Decisions Enabled:</b> Resource allocation and outlook confidence.</p>
      </div>
      <div class="fw-card">
        <h4>Efficiency Reporting Framework</h4>
        <p><b>Purpose:</b> Assess location strategy, span of control, and operating efficiency.</p>
        <p><b>Metrics Used:</b> Cost per FTE, Span of Control, Utilization.</p>
        <p><b>Leadership Decisions Enabled:</b> Location and structure decisions.</p>
      </div>
      <div class="fw-card">
        <h4>Business Case Framework</h4>
        <p><b>Purpose:</b> Quantify the case for new business initiatives.</p>
        <p><b>Metrics Used:</b> ROI, Payback, Risk-Adjusted Margin.</p>
        <p><b>Leadership Decisions Enabled:</b> Funding and prioritization calls.</p>
      </div>
      <div class="fw-card">
        <h4>KPI Governance &amp; Data Quality Framework</h4>
        <p><b>Purpose:</b> Monitor key metrics and drive data quality across the product area.</p>
        <p><b>Metrics Used:</b> Data Completeness, Metric Drift, Exception Rate.</p>
        <p><b>Leadership Decisions Enabled:</b> Trusted reporting for decision-making.</p>
      </div>
      <div class="fw-card">
        <h4>Variance &amp; Commentary Framework</h4>
        <p><b>Purpose:</b> Explain changes from prior forecasts/budgets to senior managers.</p>
        <p><b>Metrics Used:</b> FvA, Driver Variance, Bias.</p>
        <p><b>Leadership Decisions Enabled:</b> Course correction and re-forecasting.</p>
      </div>
      <div class="fw-card">
        <h4>Reporting &amp; Dashboard Design Framework</h4>
        <p><b>Purpose:</b> Deliver financial results efficiently to senior management.</p>
        <p><b>Metrics Used:</b> Refresh Cadence, Adoption, Time-to-Insight.</p>
        <p><b>Leadership Decisions Enabled:</b> Faster, self-service executive reviews.</p>
      </div>
      <div class="fw-card">
        <h4>Controls &amp; Automation Framework</h4>
        <p><b>Purpose:</b> Streamline processes and minimize cycle times.</p>
        <p><b>Metrics Used:</b> Cycle Time, Error Rate, Automation Coverage.</p>
        <p><b>Leadership Decisions Enabled:</b> Process investment and risk reduction.</p>
      </div>
      <div class="fw-card">
        <h4>Special Projects / Rapid Turnaround Playbook</h4>
        <p><b>Purpose:</b> Execute ad hoc leadership requests with little lead time.</p>
        <p><b>Metrics Used:</b> Turnaround Time, Accuracy, Stakeholder Satisfaction.</p>
        <p><b>Leadership Decisions Enabled:</b> Rapid-response support for senior management.</p>
      </div>
    </div>
  </section>

</main>

<footer>Financial Analysis (FP&amp;A) &bull; Built for executive and hiring review.</footer>

</div>

<script src="https://unpkg.com/lucide@latest"></script>
<script>
  const tabs = [
    ['overview','Overview'],
    ['fit','Role Fit Scorecard'],
    ['lab','Analysis Lab'],
    ['scenario','Forecast Simulator'],
    ['stories','Impact Stories'],
    ['playbook','Decision Playbook'],
    ['cost','Automation & Controls'],
    ['capability','Capability Center'],
    ['toolkit','FP&A Toolkit'],
  ];
  const navEl = document.getElementById('tabnav');
  tabs.forEach(([id,label])=>{
    const b = document.createElement('button');
    b.textContent = label;
    b.dataset.id = id;
    b.onclick = ()=>go(id);
    navEl.appendChild(b);
  });

  function go(id){
    document.querySelectorAll('section').forEach(s=>s.classList.toggle('active', s.id===id));
    document.querySelectorAll('nav button').forEach(b=>b.classList.toggle('active', b.dataset.id===id));
    window.scrollTo({top:0,behavior:'smooth'});
  }
  go('overview');

  function showQR(){
    alert('Deploy this file (e.g. Netlify, GitHub Pages, Vercel) to get a live URL, then generate a QR code for that URL — see chat for step-by-step instructions.');
  }

  const loadingScreen = document.getElementById('loadingScreen');
  if (loadingScreen) {
    window.addEventListener('load', () => {
      setTimeout(() => {
        loadingScreen.classList.add('hidden');
        document.body.classList.add('loaded');
      }, 1200);
    });
  }

  const particleLayer = document.getElementById('particleLayer');
  if (particleLayer) {
    for (let i = 0; i < 26; i++) {
      const p = document.createElement('span');
      p.className = 'particle';
      p.style.left = `${Math.random() * 100}%`;
      p.style.top = `${Math.random() * 100}%`;
      p.style.width = `${2 + Math.random() * 3}px`;
      p.style.height = p.style.width;
      p.style.animationDuration = `${10 + Math.random() * 16}s`;
      p.style.animationDelay = `${Math.random() * -12}s`;
      particleLayer.appendChild(p);
    }
  }

  const cursorGlow = document.getElementById('cursorGlow');
  window.addEventListener('pointermove', (e) => {
    if (!cursorGlow) return;
    cursorGlow.style.opacity = '1';
    cursorGlow.style.left = `${e.clientX}px`;
    cursorGlow.style.top = `${e.clientY}px`;
  });
  window.addEventListener('pointerleave', () => {
    if (cursorGlow) cursorGlow.style.opacity = '0';
  });

  const progressBar = document.querySelector('.scroll-progress');
  document.addEventListener('scroll', () => {
    if (!progressBar) return;
    const scrollTop = window.scrollY;
    const maxScroll = document.documentElement.scrollHeight - window.innerHeight;
    const ratio = maxScroll > 0 ? scrollTop / maxScroll : 0;
    progressBar.style.width = `${Math.min(100, Math.max(0, ratio * 100))}%`;
  });

  const revealTargets = Array.from(document.querySelectorAll('.panel, .story-card, .skill, .fw-card, .hero, .side-panel, .stat-mini, .tree-row, .out-card'));
  revealTargets.forEach((el, index) => {
    el.classList.add('reveal');
    el.style.transitionDelay = `${index * 35}ms`;
  });
  const revealObserver = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add('in-view');
        revealObserver.unobserve(entry.target);
      }
    });
  }, {threshold: 0.16});
  revealTargets.forEach((el) => revealObserver.observe(el));

  if (window.lucide) {
    window.lucide.createIcons();
  }

  // ---- Charts ----
  Chart.defaults.color = '#9caec7';
  Chart.defaults.font.family = 'Inter, "Segoe UI", system-ui, sans-serif';
  Chart.defaults.font.size = 11;
  Chart.defaults.animation.duration = 1400;
  Chart.defaults.animation.easing = 'easeOutQuart';

  const gridColor = 'rgba(255,255,255,0.08)';
  const axisOpts = {
    grid:{color:gridColor, drawBorder:false},
    ticks:{color:'#7d8da7', padding:8},
    border:{display:false},
  };

  new Chart(document.getElementById('chartGrowthMargin'), {
    type:'bubble',
    data:{datasets:[{
      label:'Accounts',
      data:[
        {x:14,y:24,r:16,label:'Alpha'},
        {x:16,y:13,r:10,label:'Nova'},
        {x:6,y:25,r:20,label:'Delta'},
        {x:3,y:18,r:9,label:'Sigma'},
        {x:11,y:18,r:8,label:'Vertex'},
      ],
      backgroundColor:['rgba(51,197,180,0.82)','rgba(240,142,123,0.78)','rgba(93,124,255,0.8)','rgba(240,142,123,0.76)','rgba(216,169,106,0.82)'],
      borderColor:'rgba(255,255,255,0.35)',
      borderWidth:1,
    }]},
    options:{
      responsive:true,maintainAspectRatio:false,
      plugins:{legend:{display:false},
        tooltip:{callbacks:{label:(c)=>c.raw.label+': Growth '+c.raw.x+'%, OM '+c.raw.y+'%'}}},
      scales:{
        x:{...axisOpts,title:{display:true,text:'Revenue Growth %',color:'#6c7b94'}},
        y:{...axisOpts,title:{display:true,text:'OM%',color:'#6c7b94'}},
      }
    }
  });

  new Chart(document.getElementById('chartBCG'), {
    type:'bubble',
    data:{datasets:[{
      label:'Portfolio',
      data:[
        {x:14,y:1.4,r:14,label:'Alpha'},
        {x:2,y:0.7,r:9,label:'Delta'},
        {x:16,y:0.9,r:8,label:'Nova'},
        {x:12,y:0.65,r:8,label:'Vertex'},
      ],
      backgroundColor:['rgba(51,197,180,0.82)','rgba(240,142,123,0.78)','rgba(93,124,255,0.8)','rgba(216,169,106,0.82)'],
      borderColor:'rgba(255,255,255,0.35)',
      borderWidth:1,
    }]},
    options:{
      responsive:true,maintainAspectRatio:false,
      plugins:{legend:{display:false},
        tooltip:{callbacks:{label:(c)=>c.raw.label+': Growth '+c.raw.x+'%, Rel. Margin '+c.raw.y}}},
      scales:{
        x:{...axisOpts,title:{display:true,text:'Growth %',color:'#6c7b94'}},
        y:{...axisOpts,title:{display:true,text:'Relative Margin',color:'#6c7b94'}},
      }
    }
  });

  new Chart(document.getElementById('chartWaterfall'), {
    type:'bar',
    data:{
      labels:['Q1 OM%','Volume','Price','Mix','Cost','Q2 OM%'],
      datasets:[{
        data:[18,4,2,-1.5,3,25.5],
        backgroundColor:['#5d7cff','#33c5b4','#33c5b4','#f08e7b','#33c5b4','#d8a96a'],
        borderRadius:6,
        borderSkipped:false,
      }]
    },
    options:{
      responsive:true,maintainAspectRatio:false,
      plugins:{legend:{display:false}},
      scales:{x:axisOpts, y:{...axisOpts,title:{display:true,text:'OM%',color:'#6c7b94'}}}
    }
  });

  // ---- Scenario simulator logic ----
  function updateScenario(){
    const g = parseFloat(document.getElementById('inGrowth').value);
    const hc = parseFloat(document.getElementById('inHC').value);
    const util = parseFloat(document.getElementById('inUtil').value);
    document.getElementById('outGrowth').textContent = g;
    document.getElementById('outHC').textContent = hc;
    document.getElementById('outUtil').textContent = util;

    const baseRevenue = 121.4;
    const revenue = baseRevenue * (1 + g/100);
    const gm = Math.min(55, 34 + (util-70)*0.25 - hc*0.15);
    const om = Math.min(35, 15 + (util-70)*0.2 - hc*0.2 + g*0.15);
    const rpp = 130000 + util*160 - hc*400;
    const opProfit = revenue * (om/100);
    const costReduction = Math.max(0, (util-75)*0.06 + (10-hc)*0.05);

    document.getElementById('valRevenue').textContent = '$'+revenue.toFixed(1)+'M';
    document.getElementById('valGM').textContent = gm.toFixed(1)+'%';
    document.getElementById('valOM').textContent = om.toFixed(1)+'%';
    document.getElementById('valRPP').textContent = '$'+Math.round(rpp).toLocaleString();
    document.getElementById('valOP').textContent = '$'+opProfit.toFixed(1)+'M';
    document.getElementById('valCR').textContent = '$'+costReduction.toFixed(1)+'M';

    const signal = document.getElementById('cfoSignal');
    if(om > 24){
      signal.textContent = 'FP&A Action Signal: Margin trajectory is expanding — supports continued growth investment.';
    } else if(om < 16){
      signal.textContent = 'FP&A Action Signal: Margin compression risk — recommend cost lever review before further hiring.';
    } else {
      signal.textContent = 'FP&A Action Signal: Current inputs support continued investment — margin trajectory remains within governed range.';
    }
  }
</script>
</body>
</html>
