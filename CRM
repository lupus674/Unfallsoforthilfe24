const express = require('express');
const cors = require('cors');
const app = express();

app.use(cors());
app.use(express.json());

let faelle = [];
let nextId = 1;

// ── CRM Web-App ausliefern ────────────────────────────────────────────────────
app.get('/', (req, res) => {
  res.send(`<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>UnfallCRM – Fallverwaltung</title>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,wght@0,300;0,400;0,600;0,700;1,300&family=DM+Mono:wght@300;400;500&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #f5f2ee; --surface: #ffffff; --surface2: #f0ece6;
  --border: #e0d8ce; --accent: #1a3a5c; --accent2: #c0392b;
  --accent3: #16a085; --text: #1a1a1a; --muted: #8a7e72;
  --green: #16a085; --orange: #e67e22; --red: #c0392b; --radius: 10px;
}
* { box-sizing: border-box; margin: 0; padding: 0; }
body { background: var(--bg); color: var(--text); font-family: 'DM Mono', monospace; font-size: 13px; min-height: 100vh; }

header {
  background: var(--accent); color: white; padding: 0 28px; height: 58px;
  display: flex; align-items: center; justify-content: space-between;
  position: sticky; top: 0; z-index: 100;
}
.logo { font-family: 'Fraunces', serif; font-size: 20px; font-weight: 600; letter-spacing: -0.5px; display: flex; align-items: center; gap: 10px; }
.logo-sub { font-family: 'DM Mono', monospace; font-size: 10px; opacity: 0.6; font-weight: 300; letter-spacing: 2px; text-transform: uppercase; }
.header-right { display: flex; align-items: center; gap: 12px; }
.live-dot-wrap { display: flex; align-items: center; gap: 6px; font-size: 10px; opacity: 0.8; letter-spacing: 1px; }
.live-dot { width: 7px; height: 7px; border-radius: 50%; background: #2ecc71; animation: blink 2s infinite; }
@keyframes blink { 0%,100%{opacity:1} 50%{opacity:0.3} }
.counter-pill { background: rgba(255,255,255,0.15); padding: 4px 12px; border-radius: 20px; font-size: 11px; }

.layout { display: flex; height: calc(100vh - 58px); }

/* SIDEBAR */
.sidebar { width: 320px; min-width: 320px; border-right: 1px solid var(--border); background: var(--surface); display: flex; flex-direction: column; overflow: hidden; }
.sidebar-header { padding: 16px; border-bottom: 1px solid var(--border); }
.sidebar-title { font-family: 'Fraunces', serif; font-size: 13px; color: var(--muted); letter-spacing: 1px; text-transform: uppercase; margin-bottom: 10px; }
.search-wrap { display: flex; align-items: center; gap: 8px; background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; padding: 7px 10px; }
.search-wrap input { background: none; border: none; outline: none; font-family: 'DM Mono', monospace; font-size: 12px; color: var(--text); flex: 1; }
.search-wrap input::placeholder { color: var(--muted); }

.fall-list { flex: 1; overflow-y: auto; }
.fall-list::-webkit-scrollbar { width: 3px; }
.fall-list::-webkit-scrollbar-thumb { background: var(--border); }

.fall-item { padding: 14px 16px; border-bottom: 1px solid var(--border); cursor: pointer; transition: all 0.15s; border-left: 3px solid transparent; }
.fall-item:hover { background: var(--surface2); }
.fall-item.active { background: #eef4fb; border-left-color: var(--accent); }
.fall-item .name { font-family: 'Fraunces', serif; font-size: 15px; font-weight: 600; margin-bottom: 3px; display: flex; justify-content: space-between; align-items: center; }
.fall-item .meta { color: var(--muted); font-size: 11px; display: flex; gap: 10px; }
.status-dot { width: 7px; height: 7px; border-radius: 50%; flex-shrink: 0; }
.status-dot.neu { background: var(--orange); }
.status-dot.beratung { background: var(--accent); }
.status-dot.bearbeitung { background: var(--accent3); }
.status-dot.abgeschlossen { background: var(--muted); }

/* MAIN */
.main { flex: 1; overflow-y: auto; background: var(--bg); }
.main::-webkit-scrollbar { width: 4px; }
.main::-webkit-scrollbar-thumb { background: var(--border); }

/* DETAIL VIEW */
.detail-header {
  background: var(--surface); border-bottom: 1px solid var(--border);
  padding: 20px 28px; display: flex; justify-content: space-between; align-items: flex-start;
}
.detail-name { font-family: 'Fraunces', serif; font-size: 26px; font-weight: 700; letter-spacing: -0.5px; margin-bottom: 4px; }
.detail-meta { display: flex; gap: 12px; color: var(--muted); font-size: 11px; flex-wrap: wrap; }
.detail-meta span { display: flex; align-items: center; gap: 4px; }

.status-select {
  font-family: 'DM Mono', monospace; font-size: 11px; padding: 6px 12px;
  border-radius: 20px; border: 1px solid var(--border); background: var(--surface2);
  cursor: pointer; outline: none; color: var(--text);
}

.detail-body { padding: 24px 28px; display: flex; flex-direction: column; gap: 18px; }

.card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 18px; }
.card-title { font-family: 'Fraunces', serif; font-size: 14px; font-weight: 600; color: var(--accent); margin-bottom: 14px; display: flex; align-items: center; gap: 8px; letter-spacing: 0.3px; }
.card-title::before { content: ''; width: 3px; height: 14px; background: var(--accent); border-radius: 2px; }

.data-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.data-field { display: flex; flex-direction: column; gap: 3px; }
.data-label { font-size: 9px; color: var(--muted); text-transform: uppercase; letter-spacing: 1.5px; }
.data-value { font-size: 13px; color: var(--text); font-weight: 500; }
.data-value.missing { color: var(--muted); font-style: italic; font-weight: 300; }
.data-field.full { grid-column: 1 / -1; }

.badge { padding: 3px 10px; border-radius: 20px; font-size: 10px; font-weight: 500; letter-spacing: 0.5px; text-transform: uppercase; }
.badge.neu { background: #fef3e2; color: var(--orange); border: 1px solid #f5cba7; }
.badge.beratung { background: #eef4fb; color: var(--accent); border: 1px solid #aed6f1; }
.badge.bearbeitung { background: #e8f8f5; color: var(--accent3); border: 1px solid #a2d9ce; }
.badge.abgeschlossen { background: var(--surface2); color: var(--muted); border: 1px solid var(--border); }

/* STATS */
.stats-row { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; padding: 20px 28px 0; }
.stat-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 14px 16px; }
.stat-num { font-family: 'Fraunces', serif; font-size: 30px; font-weight: 700; color: var(--accent); line-height: 1; margin-bottom: 4px; }
.stat-lbl { font-size: 9px; color: var(--muted); text-transform: uppercase; letter-spacing: 1.5px; }

/* EMPTY */
.empty-state { display: flex; flex-direction: column; align-items: center; justify-content: center; padding: 80px 40px; color: var(--muted); text-align: center; gap: 12px; }
.empty-icon { font-size: 56px; opacity: 0.2; }
.empty-state h3 { font-family: 'Fraunces', serif; font-size: 20px; color: var(--text); opacity: 0.4; }
.empty-state p { font-size: 12px; max-width: 260px; line-height: 1.7; }

/* TIMELINE */
.timeline { display: flex; flex-direction: column; gap: 10px; }
.tl-item { display: flex; gap: 12px; align-items: flex-start; }
.tl-dot { width: 10px; height: 10px; border-radius: 50%; background: var(--accent); flex-shrink: 0; margin-top: 3px; border: 2px solid white; box-shadow: 0 0 0 1px var(--accent); }
.tl-dot.done { background: var(--green); box-shadow: 0 0 0 1px var(--green); }
.tl-dot.pending { background: var(--border); box-shadow: 0 0 0 1px var(--border); }
.tl-content { flex: 1; }
.tl-label { font-weight: 500; font-size: 12px; }
.tl-time { font-size: 10px; color: var(--muted); margin-top: 2px; }

/* BUTTONS */
.btn { padding: 8px 16px; border: none; border-radius: 6px; cursor: pointer; font-family: 'DM Mono', monospace; font-size: 11px; font-weight: 500; transition: all 0.15s; letter-spacing: 0.3px; }
.btn-primary { background: var(--accent); color: white; }
.btn-primary:hover { background: #1e4a72; transform: translateY(-1px); }
.btn-secondary { background: var(--surface2); color: var(--text); border: 1px solid var(--border); }
.btn-secondary:hover { border-color: var(--accent); color: var(--accent); }
.btn-danger { background: transparent; color: var(--red); border: 1px solid var(--red); }
.btn-danger:hover { background: var(--red); color: white; }
.actions-row { display: flex; gap: 8px; flex-wrap: wrap; }

/* FALL TABLE */
.fall-table { width: 100%; border-collapse: collapse; }
.fall-table th { text-align: left; font-size: 9px; color: var(--muted); text-transform: uppercase; letter-spacing: 1.5px; padding: 8px 12px; border-bottom: 1px solid var(--border); font-weight: 400; }
.fall-table td { padding: 11px 12px; border-bottom: 1px solid var(--border); font-size: 12px; }
.fall-table tr { cursor: pointer; transition: background 0.1s; }
.fall-table tr:hover td { background: var(--surface2); }
.fall-table tr td:first-child { font-family: 'Fraunces', serif; font-size: 14px; font-weight: 600; }

/* TOAST */
.toast { position: fixed; bottom: 24px; right: 24px; background: var(--accent); color: white; padding: 12px 20px; border-radius: 8px; font-size: 12px; z-index: 300; transform: translateY(80px); opacity: 0; transition: all 0.25s; pointer-events: none; }
.toast.show { transform: translateY(0); opacity: 1; }

/* TERMIN BADGE */
.termin-box { background: #eef4fb; border: 1px solid #aed6f1; border-radius: 8px; padding: 12px 14px; display: flex; align-items: center; gap: 12px; }
.termin-icon { font-size: 24px; }
.termin-info h4 { font-family: 'Fraunces', serif; font-size: 14px; color: var(--accent); }
.termin-info p { font-size: 11px; color: var(--muted); margin-top: 2px; }

.separator { border: none; border-top: 1px solid var(--border); }
</style>
</head>
<body>
<header>
  <div>
    <div class="logo">⚖️ UnfallCRM</div>
    <div class="logo-sub">Fallverwaltung</div>
  </div>
  <div class="header-right">
    <div class="live-dot-wrap"><div class="live-dot"></div> LIVE</div>
    <div class="counter-pill" id="counterPill">0 Fälle</div>
  </div>
</header>

<div class="layout">
  <aside class="sidebar">
    <div class="sidebar-header">
      <div class="sidebar-title">Fälle</div>
      <div class="search-wrap">
        <span style="color:var(--muted)">🔍</span>
        <input type="text" id="searchInput" placeholder="Name, Kennzeichen..." oninput="filterFaelle()">
      </div>
    </div>
    <div class="fall-list" id="fallList"></div>
  </aside>

  <main class="main" id="mainContent"></main>
</div>

<div class="toast" id="toast"></div>

<script>
let faelle = [];
let selectedId = null;

async function loadFaelle() {
  try {
    const data = await (await fetch('/api/faelle')).json();
    const newOnes = data.filter(f => !faelle.find(x => x.id === f.id));
    faelle = data;
    renderSidebar();
    if (selectedId) {
      const f = faelle.find(x => x.id === selectedId);
      if (f) renderDetail(f);
    } else {
      renderMain();
    }
    if (newOnes.length > 0) {
      showToast('📋 Neuer Fall: ' + newOnes[0].kunde.name);
    }
  } catch(e) {}
}

function renderSidebar(filter = '') {
  const list = document.getElementById('fallList');
  const filtered = faelle.filter(f =>
    (f.kunde?.name + f.unfall?.kennzeichen_gegner + f.kunde?.email).toLowerCase().includes(filter.toLowerCase())
  );
  if (filtered.length === 0) {
    list.innerHTML = '<div style="padding:20px;text-align:center;color:var(--muted);font-size:11px;">Keine Fälle gefunden</div>';
    return;
  }
  list.innerHTML = filtered.map(f => \`
    <div class="fall-item \${f.id === selectedId ? 'active' : ''}" onclick="selectFall(\${f.id})">
      <div class="name">
        \${f.kunde?.name || 'Unbekannt'}
        <span class="status-dot \${f.status}"></span>
      </div>
      <div class="meta">
        <span>\${f.unfall?.datum || f.created_date}</span>
        <span>\${f.unfall?.ort || '—'}</span>
      </div>
    </div>
  \`).join('');
}

function filterFaelle() {
  renderSidebar(document.getElementById('searchInput').value);
}

function selectFall(id) {
  selectedId = id;
  renderSidebar(document.getElementById('searchInput').value);
  const f = faelle.find(x => x.id === id);
  if (f) renderDetail(f);
}

function renderMain() {
  const main = document.getElementById('mainContent');
  const total = faelle.length;
  const neu = faelle.filter(f => f.status === 'neu').length;
  const beratung = faelle.filter(f => f.status === 'beratung').length;
  const bearbeitung = faelle.filter(f => f.status === 'bearbeitung').length;

  if (total === 0) {
    main.innerHTML = \`
      <div class="empty-state">
        <div class="empty-icon">⚖️</div>
        <h3>Noch keine Fälle</h3>
        <p>Sobald ein Kunde anruft und der Voice Agent die Daten aufnimmt, erscheint der Fall hier automatisch.</p>
      </div>\`;
    return;
  }

  main.innerHTML = \`
    <div class="stats-row">
      <div class="stat-card"><div class="stat-num">\${total}</div><div class="stat-lbl">Gesamt</div></div>
      <div class="stat-card"><div class="stat-num" style="color:var(--orange)">\${neu}</div><div class="stat-lbl">Neu</div></div>
      <div class="stat-card"><div class="stat-num" style="color:var(--accent)">\${beratung}</div><div class="stat-lbl">Beratung</div></div>
      <div class="stat-card"><div class="stat-num" style="color:var(--accent3)">\${bearbeitung}</div><div class="stat-lbl">In Bearbeitung</div></div>
    </div>
    <div style="padding:20px 28px;">
      <div class="card">
        <div class="card-title">Alle Fälle</div>
        <table class="fall-table">
          <thead><tr>
            <th>Name</th><th>Datum</th><th>Unfallort</th><th>Kennzeichen Gegner</th><th>Status</th><th>Termin</th>
          </tr></thead>
          <tbody>
            \${faelle.map(f => \`
              <tr onclick="selectFall(\${f.id})">
                <td>\${f.kunde?.name || '—'}</td>
                <td style="color:var(--muted)">\${f.unfall?.datum || f.created_date}</td>
                <td style="color:var(--muted)">\${f.unfall?.ort || '—'}</td>
                <td style="color:var(--muted)">\${f.unfall?.kennzeichen_gegner || '—'}</td>
                <td><span class="badge \${f.status}">\${statusLabel(f.status)}</span></td>
                <td style="color:var(--muted)">\${f.termin?.datum ? f.termin.datum + ' ' + f.termin.uhrzeit : '—'}</td>
              </tr>
            \`).join('')}
          </tbody>
        </table>
      </div>
    </div>
  \`;
}

function renderDetail(f) {
  const main = document.getElementById('mainContent');
  document.getElementById('counterPill').textContent = faelle.length + ' Fälle';

  main.innerHTML = \`
    <div class="detail-header">
      <div>
        <div class="detail-name">\${f.kunde?.name || 'Unbekannter Kunde'}</div>
        <div class="detail-meta">
          <span>📋 Fall #\${String(f.id).padStart(4,'0')}</span>
          <span>📞 Via Voice Agent</span>
          <span>🕐 \${f.created_date} \${f.created_time}</span>
          \${f.kunde?.email ? '<span>✉️ ' + f.kunde.email + '</span>' : ''}
        </div>
      </div>
      <div style="display:flex;flex-direction:column;gap:8px;align-items:flex-end;">
        <select class="status-select" onchange="updateStatus(\${f.id}, this.value)">
          <option value="neu" \${f.status==='neu'?'selected':''}>Neu</option>
          <option value="beratung" \${f.status==='beratung'?'selected':''}>Beratung vereinbart</option>
          <option value="bearbeitung" \${f.status==='bearbeitung'?'selected':''}>In Bearbeitung</option>
          <option value="abgeschlossen" \${f.status==='abgeschlossen'?'selected':''}>Abgeschlossen</option>
        </select>
        <span class="badge \${f.status}">\${statusLabel(f.status)}</span>
      </div>
    </div>

    <div class="detail-body">

      \${f.termin?.datum ? \`
      <div class="termin-box">
        <div class="termin-icon">📅</div>
        <div class="termin-info">
          <h4>Beratungstermin: \${f.termin.datum} um \${f.termin.uhrzeit} Uhr</h4>
          <p>\${f.termin.notiz || 'Telefonischer Beratungstermin'}</p>
        </div>
      </div>\` : \`
      <div style="background:#fff8e1;border:1px solid #ffe082;border-radius:8px;padding:12px 14px;font-size:12px;color:#795548;">
        ⚠️ Noch kein Beratungstermin vereinbart
      </div>\`}

      <div class="card">
        <div class="card-title">Kundendaten</div>
        <div class="data-grid">
          <div class="data-field"><div class="data-label">Name</div><div class="data-value">\${f.kunde?.name || '—'}</div></div>
          <div class="data-field"><div class="data-label">Telefon</div><div class="data-value">\${f.kunde?.telefon || '—'}</div></div>
          <div class="data-field"><div class="data-label">E-Mail</div><div class="data-value">\${f.kunde?.email || '—'}</div></div>
          <div class="data-field"><div class="data-label">Adresse</div><div class="data-value">\${f.kunde?.adresse || '—'}</div></div>
          <div class="data-field"><div class="data-label">Eigenes Kennzeichen</div><div class="data-value">\${f.kunde?.kennzeichen || '—'}</div></div>
          <div class="data-field"><div class="data-label">Eigene Versicherung</div><div class="data-value">\${f.kunde?.versicherung || '—'}</div></div>
        </div>
      </div>

      <div class="card">
        <div class="card-title" style="--accent:#c0392b">Unfalldaten</div>
        <div class="data-grid">
          <div class="data-field"><div class="data-label">Datum</div><div class="data-value">\${f.unfall?.datum || '—'}</div></div>
          <div class="data-field"><div class="data-label">Uhrzeit</div><div class="data-value">\${f.unfall?.uhrzeit || '—'}</div></div>
          <div class="data-field full"><div class="data-label">Unfallort</div><div class="data-value">\${f.unfall?.ort || '—'}</div></div>
          <div class="data-field full"><div class="data-label">Unfallhergang</div><div class="data-value">\${f.unfall?.hergang || '—'}</div></div>
          <div class="data-field"><div class="data-label">Verletzte</div><div class="data-value">\${f.unfall?.verletzte || '—'}</div></div>
          <div class="data-field"><div class="data-label">Polizei gerufen</div><div class="data-value">\${f.unfall?.polizei || '—'}</div></div>
          \${f.unfall?.aktenzeichen ? '<div class="data-field"><div class="data-label">Aktenzeichen</div><div class="data-value">'+f.unfall.aktenzeichen+'</div></div>' : ''}
          <div class="data-field"><div class="data-label">Fotos gemacht</div><div class="data-value">\${f.unfall?.fotos || '—'}</div></div>
        </div>
      </div>

      <div class="card">
        <div class="card-title" style="--accent:#e67e22">Gegner / Verursacher</div>
        <div class="data-grid">
          <div class="data-field"><div class="data-label">Name</div><div class="data-value">\${f.gegner?.name || '—'}</div></div>
          <div class="data-field"><div class="data-label">Telefon</div><div class="data-value">\${f.gegner?.telefon || '—'}</div></div>
          <div class="data-field"><div class="data-label">Kennzeichen</div><div class="data-value \${!f.gegner?.kennzeichen?'missing':''}">\${f.gegner?.kennzeichen || 'nicht erfasst'}</div></div>
          <div class="data-field"><div class="data-label">Versicherung</div><div class="data-value \${!f.gegner?.versicherung?'missing':''}">\${f.gegner?.versicherung || 'nicht erfasst'}</div></div>
          <div class="data-field"><div class="data-label">Versicherungsnummer</div><div class="data-value \${!f.gegner?.versicherungsnummer?'missing':''}">\${f.gegner?.versicherungsnummer || 'nicht erfasst'}</div></div>
          <div class="data-field"><div class="data-label">Führerscheinnummer</div><div class="data-value \${!f.gegner?.fuehrerschein?'missing':''}">\${f.gegner?.fuehrerschein || 'nicht erfasst'}</div></div>
        </div>
      </div>

      <div class="actions-row">
        <button class="btn btn-primary" onclick="updateStatus(\${f.id},'bearbeitung')">✓ In Bearbeitung setzen</button>
        <button class="btn btn-secondary" onclick="updateStatus(\${f.id},'abgeschlossen')">Abschließen</button>
        <button class="btn btn-danger" onclick="deleteFall(\${f.id})">Löschen</button>
      </div>

    </div>
  \`;
}

function statusLabel(s) {
  return { neu: 'Neu', beratung: 'Beratung', bearbeitung: 'In Bearbeitung', abgeschlossen: 'Abgeschlossen' }[s] || s;
}

async function updateStatus(id, status) {
  await fetch('/api/fall/' + id + '/status', { method: 'PUT', headers: {'Content-Type':'application/json'}, body: JSON.stringify({status}) });
  const f = faelle.find(x => x.id === id);
  if (f) { f.status = status; renderSidebar(); renderDetail(f); }
  showToast('Status aktualisiert');
}

async function deleteFall(id) {
  if (!confirm('Fall wirklich löschen?')) return;
  await fetch('/api/fall/' + id, { method: 'DELETE' });
  faelle = faelle.filter(x => x.id !== id);
  selectedId = null;
  renderSidebar();
  renderMain();
  showToast('Fall gelöscht');
}

function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg; t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2500);
}

loadFaelle();
setInterval(loadFaelle, 4000);
document.getElementById('counterPill').textContent = '0 Fälle';
</script>
</body>
</html>`);
});

// ── Vapi ruft diese URL auf nach dem Gespräch ─────────────────────────────────
app.post('/api/fall', (req, res) => {
  console.log('📞 Neuer Fall eingehend:', JSON.stringify(req.body, null, 2));

  const args =
    req.body?.message?.toolCallList?.[0]?.function?.arguments ||
    req.body?.tool_call?.function?.arguments ||
    req.body;

  const parsed = typeof args === 'string' ? JSON.parse(args) : args;

  const now = new Date();
  const fall = {
    id: nextId++,
    status: 'neu',
    created_date: now.toLocaleDateString('de-DE'),
    created_time: now.toLocaleTimeString('de-DE', { hour: '2-digit', minute: '2-digit' }),
    kunde: {
      name:         parsed.kunde_name         || parsed.name         || 'Unbekannt',
      telefon:      parsed.kunde_telefon      || parsed.telefon      || '',
      email:        parsed.kunde_email        || parsed.email        || '',
      adresse:      parsed.kunde_adresse      || '',
      kennzeichen:  parsed.kunde_kennzeichen  || '',
      versicherung: parsed.kunde_versicherung || '',
    },
    unfall: {
      datum:      parsed.unfall_datum    || now.toLocaleDateString('de-DE'),
      uhrzeit:    parsed.unfall_uhrzeit  || '',
      ort:        parsed.unfall_ort      || '',
      hergang:    parsed.unfall_hergang  || '',
      verletzte:  parsed.verletzte       || 'Nein',
      polizei:    parsed.polizei_gerufen || 'Nein',
      aktenzeichen: parsed.aktenzeichen  || '',
      fotos:      parsed.fotos_gemacht   || 'Unbekannt',
    },
    gegner: {
      name:               parsed.gegner_name               || '',
      telefon:            parsed.gegner_telefon             || '',
      kennzeichen:        parsed.gegner_kennzeichen         || '',
      versicherung:       parsed.gegner_versicherung        || '',
      versicherungsnummer: parsed.gegner_versicherungsnummer || '',
      fuehrerschein:      parsed.gegner_fuehrerschein       || '',
    },
    termin: {
      datum:   parsed.termin_datum   || '',
      uhrzeit: parsed.termin_uhrzeit || '',
      notiz:   parsed.termin_notiz   || '',
    }
  };

  faelle.unshift(fall);
  console.log('✅ Fall gespeichert:', fall.kunde.name, '| Fall #' + fall.id);

  res.json({
    result: `Vielen Dank ${fall.kunde.name}! Ihr Fall wurde erfolgreich aufgenommen. Sie erhalten in Kürze eine Zusammenfassung per E-Mail und wir melden uns zum vereinbarten Beratungstermin. Auf Wiederhören!`
  });
});

// ── Alle Fälle abrufen ────────────────────────────────────────────────────────
app.get('/api/faelle', (req, res) => res.json(faelle));

// ── Status aktualisieren ──────────────────────────────────────────────────────
app.put('/api/fall/:id/status', (req, res) => {
  const f = faelle.find(x => x.id === parseInt(req.params.id));
  if (f) { f.status = req.body.status; res.json({ ok: true }); }
  else res.status(404).json({ error: 'Nicht gefunden' });
});

// ── Fall löschen ──────────────────────────────────────────────────────────────
app.delete('/api/fall/:id', (req, res) => {
  faelle = faelle.filter(x => x.id !== parseInt(req.params.id));
  res.json({ ok: true });
});

// ── Health Check ──────────────────────────────────────────────────────────────
app.get('/health', (req, res) => res.json({ status: 'online', faelle: faelle.length }));

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`⚖️  UnfallCRM läuft auf Port ${PORT}`));
