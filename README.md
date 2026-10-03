# OmniResolve-AI-Enterprise-Autonomous-Operations-Engine
OmniResolve AI is an autonomous incident triage and operations engine built with n8n AI Agents, Gemini 2.5 Pro, and Java Spring Boot microservices. It ingests omnichannel alerts, concurrently correlates scattered data across PostgreSQL ledgers, server logs, and Pinecone RAG policies, and autonomously executes self-healing runbooks .
<!DOCTYPE html>
<html lang="en" data-theme="dark">
<head>
  <meta charset="UTF-8" />
  <title>OmniResolve AI – Operations Engine</title>
  <style>
    :root[data-theme="dark"] { --bg:#0b1116; --panel:#111921; --border:#1e2a36; --text:#e6edf3; --muted:#8b949e; --accent:#19a974; }
    :root[data-theme="light"] { --bg:#f5f7f9; --panel:#ffffff; --border:#e2e8f0; --text:#0f172a; --muted:#64748b; --accent:#0f8a5f; }
    * { box-sizing:border-box; margin:0; padding:0; font-family:system-ui, sans-serif; }
    body { display:flex; height:100vh; background:var(--bg); color:var(--text); }
    .sidebar { width:220px; background:var(--panel); border-right:1px solid var(--border); padding:20px; display:flex; flex-direction:column; justify-content:space-between; }
    .nav-item { display:block; padding:10px 12px; border-radius:8px; color:var(--muted); cursor:pointer; margin-bottom:4px; }
    .nav-item.active { background:rgba(25,169,116,0.15); color:var(--accent); font-weight:600; }
    .main { flex:1; padding:28px; overflow-y:auto; }
    .kpi-grid { display:grid; grid-template-columns:repeat(4, 1fr); gap:14px; margin:20px 0; }
    .card { background:var(--panel); border:1px solid var(--border); border-radius:12px; padding:16px; margin-bottom:14px; }
    .kpi-val { font-size:26px; font-weight:700; margin:6px 0; }
    .row { display:flex; justify-content:space-between; align-items:center; padding:12px 0; border-bottom:1px solid var(--border); font-size:13px; }
    .btn { background:var(--accent); color:#fff; border:none; padding:8px 14px; border-radius:8px; cursor:pointer; font-weight:600; }
    .section { display:none; } .section.active { display:block; }
  </style>
</head>
<body>
  <aside class="sidebar">
    <div>
      <h3>OmniResolve AI</h3>
      <small style="color:var(--muted)">OPERATIONS ENGINE</small>
      <div style="margin-top:20px">
        <a class="nav-item active" onclick="tab('overview',this)">Overview</a>
        <a class="nav-item" onclick="tab('incidents',this)">Incidents (5)</a>
        <a class="nav-item" onclick="tab('agents',this)">Agents (6)</a>
        <a class="nav-item" onclick="tab('runbooks',this)">Runbooks</a>
        <a class="nav-item" onclick="tab('audit',this)">Audit trail</a>
      </div>
    </div>
    <button class="btn" onclick="toggleTheme()">Switch Theme</button>
  </aside>
  <main class="main">
    <div style="display:flex; justify-content:space-between; align-items:center;">
      <div><h1 id="title">Overview</h1><p style="color:var(--muted)">Autonomous operations across 214 services</p></div>
      <button class="btn" onclick="simulate()">+ Simulate incident</button>
    </div>
    <section id="overview" class="section active">
      <div class="kpi-grid">
        <div class="card"><small>AUTO-RESOLVED TODAY</small><div class="kpi-val">83.0%</div><small style="color:var(--accent)">+4.2 pts vs last week</small></div>
        <div class="card"><small>MEAN TIME TO RESOLVE</small><div class="kpi-val">6.4 min</div><small style="color:var(--accent)">-71% vs manual baseline</small></div>
        <div class="card"><small>OPEN INCIDENTS</small><div class="kpi-val">5</div><small>2 waiting on a decision</small></div>
        <div class="card"><small>ENGINEER HOURS SAVED</small><div class="kpi-val">312 h</div><small>this month · about ₹18.4 L</small></div>
      </div>
      <div class="card">
        <strong>Needs a decision (HITL Approval)</strong>
        <div class="row"><span>Roll back last deploy · INC-4181 · checkout-api (81% confidence)</span><button class="btn" onclick="this.innerText='Approved ✓'">Approve</button></div>
      </div>
    </section>
    <section id="incidents" class="section"><div class="card" id="inc-list">
      <div class="row"><span>[P1] Latency spike on checkout-api (INC-4181 · 81%)</span><span>Needs approval · Diagnosis</span></div>
      <div class="row"><span>[P1] Payment gateway timeouts (INC-4182 · 91%)</span><span>Analyzing · Diagnosis</span></div>
      <div class="row"><span>[P2] Replica lag on orders-db (INC-4185 · 91%)</span><span>Resolved · Capacity</span></div>
    </div></section>
    <section id="agents" class="section"><div class="card">
      <div class="row"><span>Triage Agent (1,284 handled · 99.1% · 1.8s)</span><strong>Mode: Act</strong></div>
      <div class="row"><span>Diagnosis Agent (612 handled · 93.4% · 42s)</span><strong>Mode: Act</strong></div>
      <div class="row"><span>Remediation Agent (437 handled · 96.8% · 3.1m)</span><strong>Mode: Suggest</strong></div>
      <div class="row"><span>Compliance Agent (437 handled · 100% · 0.9s)</span><strong>Mode: Act</strong></div>
      <div class="row"><span>Capacity Agent (96 handled · 97.9% · 12s)</span><strong>Mode: Suggest</strong></div>
      <div class="row"><span>Comms Agent (823 handled · 98.6% · 4s)</span><strong>Mode: Act</strong></div>
    </div></section>
    <section id="runbooks" class="section"><div class="card">
      <div class="row"><span>Restart unhealthy pods (pod.restart_count > 3 within 10m)</span><span>97.2% success</span></div>
      <div class="row"><span>Roll back last deploy (error_rate > 5% within 15m)</span><span>94.1% success</span></div>
      <div class="row"><span>Scale out database replicas (replica_lag > 30s and cpu > 85%)</span><span>98.5% success</span></div>
    </div></section>
    <section id="audit" class="section"><div class="card" id="audit-list">
      <div class="row"><span>19:44 · Triage opened INC-4191 · Payment gateway timeouts</span></div>
      <div class="row"><span>19:44 · Remediation fix executed on INC-4184</span></div>
      <div class="row"><span>19:41 · Compliance approved the rollback plan for INC-4181</span></div>
      <div class="row"><span>19:38 · Diagnosis traced checkout-api latency to release 4.12.1</span></div>
    </div></section>
  </main>
  <script>
    function tab(id, el) {
      document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
      document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
      document.getElementById(id).classList.add('active'); el.classList.add('active');
      document.getElementById('title').innerText = el.innerText;
    }
    function toggleTheme() {
      const h = document.documentElement;
      h.setAttribute('data-theme', h.getAttribute('data-theme') === 'dark' ? 'light' : 'dark');
    }
    function simulate() {
      const id = 'INC-' + Math.floor(4200 + Math.random() * 800);
      document.getElementById('audit-list').innerHTML = `<div class="row"><span>Just now · Remediation auto-resolved ${id} via n8n webhook</span></div>` + document.getElementById('audit-list').innerHTML;
      alert('Simulated ' + id + ' resolved autonomously!');
    }
  </script>
</body>
</html>
