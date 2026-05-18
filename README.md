# mez042.github.io.

<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Análisis Estadístico — Examen 2do Parcial</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Syne:wght@400;600;800&display=swap');

  :root {
    --bg: #0a0a0f;
    --surface: #13131c;
    --surface2: #1c1c2a;
    --border: #2a2a3e;
    --accent: #7c3aed;
    --accent2: #06d6a0;
    --accent3: #f72585;
    --accent4: #ffd60a;
    --text: #e8e8f0;
    --muted: #7070a0;
    --blanco: #a78bfa;
    --amarillo: #ffd60a;
    --verde: #06d6a0;
    --na: #ef4444;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Syne', sans-serif;
    overflow-x: hidden;
    line-height: 1.6;
  }

  /* BG grid */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(124,58,237,.05) 1px, transparent 1px),
      linear-gradient(90deg, rgba(124,58,237,.05) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }

  /* HEADER */
  header {
    position: relative;
    padding: 80px 40px 60px;
    text-align: center;
    border-bottom: 1px solid var(--border);
    overflow: hidden;
  }
  header::after {
    content: '';
    position: absolute;
    bottom: -1px; left: 50%;
    transform: translateX(-50%);
    width: 200px; height: 2px;
    background: linear-gradient(90deg, transparent, var(--accent), var(--accent2), transparent);
  }
  .tag {
    display: inline-block;
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    letter-spacing: 3px;
    color: var(--accent2);
    border: 1px solid var(--accent2);
    padding: 4px 12px;
    border-radius: 2px;
    margin-bottom: 20px;
    text-transform: uppercase;
  }
  h1 {
    font-size: clamp(2rem, 5vw, 4rem);
    font-weight: 800;
    letter-spacing: -2px;
    background: linear-gradient(135deg, #fff 30%, var(--accent), var(--accent2));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    margin-bottom: 16px;
  }
  .subtitle {
    color: var(--muted);
    font-size: 1rem;
    font-family: 'Space Mono', monospace;
  }

  /* NAV */
  nav {
    position: sticky;
    top: 0;
    z-index: 100;
    background: rgba(10,10,15,.9);
    backdrop-filter: blur(12px);
    border-bottom: 1px solid var(--border);
    padding: 0 40px;
    display: flex;
    gap: 0;
    overflow-x: auto;
  }
  nav a {
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: var(--muted);
    text-decoration: none;
    padding: 16px 20px;
    border-bottom: 2px solid transparent;
    transition: all .2s;
    white-space: nowrap;
  }
  nav a:hover { color: var(--text); border-color: var(--accent); }

  /* MAIN LAYOUT */
  main {
    position: relative;
    z-index: 1;
    max-width: 1300px;
    margin: 0 auto;
    padding: 0 24px 80px;
  }

  /* SECTION */
  section {
    padding: 60px 0 0;
  }
  .section-label {
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    letter-spacing: 4px;
    color: var(--accent);
    text-transform: uppercase;
    margin-bottom: 8px;
  }
  .section-title {
    font-size: clamp(1.4rem, 3vw, 2.2rem);
    font-weight: 800;
    letter-spacing: -1px;
    margin-bottom: 32px;
    color: #fff;
  }
  .section-title span { color: var(--accent2); }

  /* CARDS */
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 28px;
    position: relative;
    overflow: hidden;
  }
  .card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
  }

  /* STATS ROW */
  .stats-row {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 16px;
    margin-bottom: 40px;
  }
  .stat-card {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 24px 20px;
    text-align: center;
    transition: transform .2s, border-color .2s;
  }
  .stat-card:hover { transform: translateY(-3px); border-color: var(--accent); }
  .stat-value {
    font-family: 'Space Mono', monospace;
    font-size: 2rem;
    font-weight: 700;
    margin-bottom: 6px;
  }
  .stat-label {
    font-size: .75rem;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: var(--muted);
  }
  .stat-sub {
    font-family: 'Space Mono', monospace;
    font-size: .7rem;
    color: var(--accent2);
    margin-top: 4px;
  }

  /* GRIDS */
  .two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 24px; }
  .three-col { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 24px; }
  @media(max-width:900px) { .two-col, .three-col { grid-template-columns: 1fr; } }

  /* TABLE */
  .tbl-wrap { overflow-x: auto; }
  table {
    width: 100%;
    border-collapse: collapse;
    font-family: 'Space Mono', monospace;
    font-size: .78rem;
  }
  thead tr {
    background: var(--surface2);
    border-bottom: 2px solid var(--accent);
  }
  th {
    padding: 12px 16px;
    text-align: left;
    letter-spacing: 2px;
    font-size: .7rem;
    text-transform: uppercase;
    color: var(--accent2);
  }
  td { padding: 10px 16px; border-bottom: 1px solid var(--border); }
  tbody tr:hover { background: rgba(124,58,237,.08); }
  .badge {
    display: inline-block;
    padding: 2px 10px;
    border-radius: 99px;
    font-size: .7rem;
    font-weight: 700;
  }
  .badge-blanco { background: rgba(167,139,250,.15); color: var(--blanco); }
  .badge-amarillo { background: rgba(255,214,10,.15); color: var(--amarillo); }
  .badge-verde { background: rgba(6,214,160,.15); color: var(--verde); }
  .badge-na { background: rgba(239,68,68,.15); color: var(--na); }

  /* CHART CONTAINERS */
  .chart-wrap {
    position: relative;
    width: 100%;
    height: 300px;
  }
  .chart-wrap-lg { height: 380px; }

  /* COLOR SWATCHES */
  .legend {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
    margin-bottom: 16px;
  }
  .legend-item {
    display: flex;
    align-items: center;
    gap: 8px;
    font-family: 'Space Mono', monospace;
    font-size: .75rem;
  }
  .swatch { width: 12px; height: 12px; border-radius: 2px; }

  /* FORMULA BLOCK */
  .formula {
    background: var(--surface2);
    border-left: 3px solid var(--accent);
    border-radius: 0 6px 6px 0;
    padding: 14px 20px;
    font-family: 'Space Mono', monospace;
    font-size: .78rem;
    color: var(--accent2);
    margin-bottom: 16px;
  }

  /* DIVIDER */
  .divider {
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--border), transparent);
    margin: 48px 0 0;
  }

  /* FOOTER */
  footer {
    text-align: center;
    padding: 40px;
    border-top: 1px solid var(--border);
    font-family: 'Space Mono', monospace;
    font-size: .75rem;
    color: var(--muted);
    position: relative;
    z-index: 1;
  }

  /* ANIMATE IN */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(24px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  .card, .stat-card { animation: fadeUp .5s ease both; }
</style>
</head>
<body>

<header>
  <div class="tag">Examen 2do Parcial — Estadística</div>
  <h1>Análisis Estadístico<br>Completo</h1>
  <p class="subtitle">peso · altura · velocidad · color — n = 28 observaciones</p>
</header>

<nav>
  <a href="#datos">Datos</a>
  <a href="#frecuencias">Frecuencias</a>
  <a href="#acumuladas">Acumuladas</a>
  <a href="#poligono">Polígono</a>
  <a href="#medidas">Media · Mediana · Moda</a>
</nav>

<main>

  <!-- ═══ SECCIÓN: DATOS CRUDOS ═══ -->
  <section id="datos">
    <p class="section-label">// 01</p>
    <h2 class="section-title">Conjunto de <span>Datos</span></h2>

    <!-- KPI cards -->
    <div class="stats-row" id="kpi-cards"></div>

    <div class="card">
      <div class="tbl-wrap">
        <table id="raw-table">
          <thead>
            <tr>
              <th>#</th><th>Peso (kg)</th><th>Altura (cm)</th>
              <th>Velocidad (km/h)</th><th>Color</th>
            </tr>
          </thead>
          <tbody></tbody>
        </table>
      </div>
    </div>
  </section>

  <div class="divider"></div>

  <!-- ═══ SECCIÓN: FRECUENCIAS ═══ -->
  <section id="frecuencias">
    <p class="section-label">// 02</p>
    <h2 class="section-title">Frecuencias <span>Absolutas & Relativas</span></h2>

    <div class="formula">fi = cantidad de veces que aparece cada categoría &nbsp;|&nbsp; fri = fi / n &nbsp;|&nbsp; n = 28</div>

    <!-- Tablas de frecuencias -->
    <div class="two-col" style="margin-bottom:32px">
      <div class="card">
        <p style="font-size:.75rem;letter-spacing:3px;text-transform:uppercase;color:var(--muted);margin-bottom:16px">Tabla — Color</p>
        <table id="freq-color-table">
          <thead><tr><th>Color</th><th>fi</th><th>fri</th><th>fri%</th></tr></thead>
          <tbody></tbody>
        </table>
      </div>
      <div class="card">
        <p style="font-size:.75rem;letter-spacing:3px;text-transform:uppercase;color:var(--muted);margin-bottom:16px">Tabla — Velocidad (intervalos)</p>
        <table id="freq-vel-table">
          <thead><tr><th>Intervalo</th><th>fi</th><th>fri</th><th>fri%</th></tr></thead>
          <tbody></tbody>
        </table>
      </div>
    </div>

    <!-- Gráficas -->
    <div class="two-col">
      <div class="card">
        <p style="font-size:.75rem;letter-spacing:3px;text-transform:uppercase;color:var(--muted);margin-bottom:16px">Gráfica de Barras — Frecuencias Absolutas (Color)</p>
        <div class="chart-wrap"><canvas id="barChart"></canvas></div>
      </div>
      <div class="card">
        <p style="font-size:.75rem;letter-spacing:3px;text-transform:uppercase;color:var(--muted);margin-bottom:16px">Diagrama de Pastel — Frecuencias Relativas (Color)</p>
        <div class="chart-wrap"><canvas id="pieChart"></canvas></div>
      </div>
    </div>

    <!-- Barras de velocidad por intervalos -->
    <div class="card" style="margin-top:24px">
      <p style="font-size:.75rem;letter-spacing:3px;text-transform:uppercase;color:var(--muted);margin-bottom:16px">Gráfica de Barras — Frecuencias Absolutas (Velocidad por intervalos)</p>
      <div class="chart-wrap chart-wrap-lg"><canvas id="barVelChart"></canvas></div>
    </div>
  </section>

  <div class="divider"></div>

  <!-- ═══ SECCIÓN: ACUMULADAS ═══ -->
  <section id="acumuladas">
    <p class="section-label">// 03</p>
    <h2 class="section-title">Frecuencias <span>Acumuladas</span></h2>

    <div class="formula">Fi = Σ fi (acumulada absoluta) &nbsp;|&nbsp; Fri = Σ fri (acumulada relativa)</div>

    <div class="two-col">
      <div class="card">
        <p style="font-size:.75rem;letter-spacing:3px;text-transform:uppercase;color:var(--muted);margin-bottom:16px">Tabla Acumulada — Velocidad</p>
        <table id="acum-table">
          <thead><tr><th>Intervalo</th><th>fi</th><th>Fi</th><th>fri%</th><th>Fri%</th></tr></thead>
          <tbody></tbody>
        </table>
      </div>
      <div class="card">
        <p style="font-size:.75rem;letter-spacing:3px;text-transform:uppercase;color:var(--muted);margin-bottom:16px">Ojiva — Frecuencia Acumulada</p>
        <div class="chart-wrap"><canvas id="acumChart"></canvas></div>
      </div>
    </div>
  </section>

  <div class="divider"></div>

  <!-- ═══ SECCIÓN: POLÍGONO ═══ -->
  <section id="poligono">
    <p class="section-label">// 04</p>
    <h2 class="section-title">Polígono de <span>Frecuencias</span></h2>

    <div class="card">
      <div class="chart-wrap chart-wrap-lg"><canvas id="polyChart"></canvas></div>
    </div>
  </section>

  <div class="divider"></div>

  <!-- ═══ SECCIÓN: MEDIDAS ═══ -->
  <section id="medidas">
    <p class="section-label">// 05</p>
    <h2 class="section-title">Media · Mediana · <span>Moda</span></h2>

    <div class="three-col" id="medidas-cards" style="margin-bottom:32px"></div>

    <div class="two-col">
      <div class="card" id="medidas-peso">
        <p style="font-size:.75rem;letter-spacing:3px;text-transform:uppercase;color:var(--muted);margin-bottom:16px">Distribución — Peso</p>
        <div class="chart-wrap"><canvas id="pesoChart"></canvas></div>
      </div>
      <div class="card" id="medidas-altura">
        <p style="font-size:.75rem;letter-spacing:3px;text-transform:uppercase;color:var(--muted);margin-bottom:16px">Distribución — Altura</p>
        <div class="chart-wrap"><canvas id="alturaChart"></canvas></div>
      </div>
    </div>
  </section>

</main>

<footer>
  Análisis Estadístico Completo &nbsp;·&nbsp; Examen 2do Parcial &nbsp;·&nbsp; n=28 observaciones
</footer>

<script>
// ══════════════════════════════════════════════
// DATOS
// ══════════════════════════════════════════════
const raw = [
  {peso:7.2, altura:50, vel:10.3, color:'Blanco'},
  {peso:8.5, altura:66, vel:10.3, color:'Amarillo'},
  {peso:9.8, altura:73, vel:10.2, color:'Verde'},
  {peso:6.5, altura:72, vel:16.4, color:'Verde'},
  {peso:7.5, altura:81, vel:18.8, color:'Verde'},
  {peso:10.1, altura:73, vel:19.7, color:'Verde'},
  {peso:11, altura:66, vel:15.6, color:'Blanco'},
  {peso:11, altura:75, vel:21.2, color:'Amarillo'},
  {peso:11.1, altura:70, vel:22.6, color:'NA'},
  {peso:11.2, altura:75, vel:19.9, color:'Blanco'},
  {peso:11.3, altura:69, vel:24.2, color:'Amarillo'},
  {peso:11.4, altura:76, vel:21, color:'Blanco'},
  {peso:11.4, altura:76, vel:21.4, color:'Verde'},
  {peso:11.7, altura:69, vel:21.3, color:'Verde'},
  {peso:12, altura:75, vel:null, color:'Amarillo'},
  {peso:12.9, altura:64, vel:22.2, color:'Amarillo'},
  {peso:12.9, altura:55, vel:33.8, color:'Blanco'},
  {peso:10.3, altura:76, vel:27.4, color:'Amarillo'},
  {peso:9.7, altura:71, vel:25.7, color:'Verde'},
  {peso:10.8, altura:64, vel:24.9, color:'Verde'},
  {peso:11, altura:78, vel:23.1, color:'Amarillo'},
  {peso:10.2, altura:70, vel:31.7, color:'Amarillo'},
  {peso:10.5, altura:74, vel:36.3, color:'Verde'},
  {peso:6.5, altura:72, vel:38.3, color:'Verde'},
  {peso:6.3, altura:77, vel:42.6, color:'Verde'},
  {peso:7.3, altura:51, vel:55.4, color:'Blanco'},
  {peso:7.5, altura:62, vel:null, color:'Blanco'},
  {peso:7.9, altura:60, vel:58.3, color:'Amarillo'},
  {peso:8.2, altura:70, vel:null, color:'Verde'},
];

const n = raw.length;
const COLORS = {
  Blanco:  { bg:'rgba(167,139,250,.7)',  border:'#a78bfa' },
  Amarillo:{ bg:'rgba(255,214,10,.7)',   border:'#ffd60a' },
  Verde:   { bg:'rgba(6,214,160,.7)',    border:'#06d6a0' },
  NA:      { bg:'rgba(239,68,68,.7)',    border:'#ef4444' },
};

// ══════════════════════════════════════════════
// HELPERS
// ══════════════════════════════════════════════
const mean  = arr => arr.reduce((a,b)=>a+b,0)/arr.length;
const sort  = arr => [...arr].sort((a,b)=>a-b);
const median = arr => {
  const s = sort(arr); const m = Math.floor(s.length/2);
  return s.length%2===0 ? (s[m-1]+s[m])/2 : s[m];
};
const mode  = arr => {
  const freq={}; arr.forEach(x=>freq[x]=(freq[x]||0)+1);
  const max=Math.max(...Object.values(freq));
  return Object.keys(freq).filter(k=>freq[k]===max).map(Number);
};
const round2 = x => Math.round(x*100)/100;

// ══════════════════════════════════════════════
// TABLA CRUDA
// ══════════════════════════════════════════════
const tbody = document.querySelector('#raw-table tbody');
raw.forEach((r,i)=>{
  const cls = r.color==='NA'?'na':r.color.toLowerCase();
  tbody.innerHTML += `<tr>
    <td style="color:var(--muted)">${i+1}</td>
    <td>${r.peso}</td><td>${r.altura}</td>
    <td>${r.vel??'—'}</td>
    <td><span class="badge badge-${cls}">${r.color}</span></td>
  </tr>`;
});

// ══════════════════════════════════════════════
// KPI
// ══════════════════════════════════════════════
const pesos   = raw.map(r=>r.peso);
const alturas = raw.map(r=>r.altura);
const vels    = raw.filter(r=>r.vel!==null).map(r=>r.vel);

const kpis = [
  {label:'Observaciones', val:n, sub:'total', color:'var(--accent)'},
  {label:'Peso promedio', val:round2(mean(pesos))+'kg', sub:`med:${round2(median(pesos))}kg`, color:'var(--accent2)'},
  {label:'Altura promedio', val:round2(mean(alturas))+'cm', sub:`med:${round2(median(alturas))}cm`, color:'var(--accent3)'},
  {label:'Vel. promedio', val:round2(mean(vels))+'km/h', sub:`n válidos:${vels.length}`, color:'var(--accent4)'},
  {label:'Vel. máxima', val:Math.max(...vels)+'km/h', sub:'', color:'var(--accent)'},
  {label:'Vel. mínima', val:Math.min(...vels)+'km/h', sub:'', color:'var(--accent2)'},
];
const kpiDiv = document.getElementById('kpi-cards');
kpis.forEach(k=>{
  kpiDiv.innerHTML += `<div class="stat-card">
    <div class="stat-value" style="color:${k.color}">${k.val}</div>
    <div class="stat-label">${k.label}</div>
    ${k.sub?`<div class="stat-sub">${k.sub}</div>`:''}
  </div>`;
});

// ══════════════════════════════════════════════
// FRECUENCIAS COLOR
// ══════════════════════════════════════════════
const colorCounts = {};
raw.forEach(r=>colorCounts[r.color]=(colorCounts[r.color]||0)+1);
const colorKeys = ['Blanco','Amarillo','Verde','NA'];

const freqColorTbody = document.querySelector('#freq-color-table tbody');
colorKeys.forEach(c=>{
  const fi = colorCounts[c]||0;
  const fri = round2(fi/n);
  const cls = c==='NA'?'na':c.toLowerCase();
  freqColorTbody.innerHTML += `<tr>
    <td><span class="badge badge-${cls}">${c}</span></td>
    <td>${fi}</td><td>${fri}</td><td>${(fri*100).toFixed(1)}%</td>
  </tr>`;
});

// ══════════════════════════════════════════════
// INTERVALOS VELOCIDAD
// ══════════════════════════════════════════════
const velData = vels;
const velMin = Math.floor(Math.min(...velData));
const velMax = Math.ceil(Math.max(...velData));
const k = Math.ceil(1+3.322*Math.log10(velData.length)); // Sturges
const h = Math.ceil((velMax-velMin)/k);

const intervals = [];
for(let i=0;i<k;i++){
  const lo = velMin + i*h;
  const hi = lo + h;
  const fi = velData.filter(v=>v>=lo && (i===k-1?v<=hi:v<hi)).length;
  intervals.push({lo,hi,fi,label:`[${lo}-${hi})`});
}

const freqVelTbody = document.querySelector('#freq-vel-table tbody');
intervals.forEach(iv=>{
  const fri = round2(iv.fi/n);
  freqVelTbody.innerHTML += `<tr>
    <td style="font-family:'Space Mono',monospace">${iv.label}</td>
    <td>${iv.fi}</td><td>${fri}</td><td>${(fri*100).toFixed(1)}%</td>
  </tr>`;
});

// ══════════════════════════════════════════════
// FRECUENCIAS ACUMULADAS
// ══════════════════════════════════════════════
let FiAcc=0, FriAcc=0;
const acumRows = intervals.map(iv=>{
  FiAcc += iv.fi;
  FriAcc += iv.fi/n;
  return {...iv, Fi:FiAcc, Fri:round2(FriAcc)};
});

const acumTbody = document.querySelector('#acum-table tbody');
acumRows.forEach(r=>{
  acumTbody.innerHTML += `<tr>
    <td style="font-family:'Space Mono',monospace">${r.label}</td>
    <td>${r.fi}</td><td>${r.Fi}</td>
    <td>${(r.fi/n*100).toFixed(1)}%</td>
    <td>${(r.Fri*100).toFixed(1)}%</td>
  </tr>`;
});

// ══════════════════════════════════════════════
// MEDIDAS CENTRALES
// ══════════════════════════════════════════════
const medidasData = [
  {var:'Peso', arr:pesos, unit:'kg'},
  {var:'Altura', arr:alturas, unit:'cm'},
  {var:'Velocidad', arr:vels, unit:'km/h'},
];
const medCardsDiv = document.getElementById('medidas-cards');
medidasData.forEach((d,idx)=>{
  const colors=['var(--accent)','var(--accent2)','var(--accent3)'];
  const med = round2(mean(d.arr));
  const mdn = round2(median(d.arr));
  const mod = mode(d.arr).map(x=>round2(x)).join(', ');
  medCardsDiv.innerHTML += `<div class="card" style="--t:${colors[idx]}">
    <div style="font-size:.7rem;letter-spacing:3px;text-transform:uppercase;color:var(--muted);margin-bottom:16px">${d.var} (${d.unit})</div>
    <table style="width:100%;font-family:'Space Mono',monospace;font-size:.8rem">
      <tr><td style="color:var(--muted);padding:8px 0;border-bottom:1px solid var(--border)">Media</td>
          <td style="text-align:right;color:${colors[idx]};padding:8px 0;border-bottom:1px solid var(--border);font-weight:700">${med}</td></tr>
      <tr><td style="color:var(--muted);padding:8px 0;border-bottom:1px solid var(--border)">Mediana</td>
          <td style="text-align:right;color:${colors[idx]};padding:8px 0;border-bottom:1px solid var(--border);font-weight:700">${mdn}</td></tr>
      <tr><td style="color:var(--muted);padding:8px 0">Moda</td>
          <td style="text-align:right;color:${colors[idx]};padding:8px 0;font-weight:700">${mod}</td></tr>
    </table>
  </div>`;
});

// ══════════════════════════════════════════════
// CHART.JS CONFIG GLOBAL
// ══════════════════════════════════════════════
Chart.defaults.color = '#7070a0';
Chart.defaults.borderColor = '#2a2a3e';
Chart.defaults.font.family = "'Space Mono', monospace";
Chart.defaults.font.size = 11;

// ── BAR COLOR ──
new Chart(document.getElementById('barChart'), {
  type: 'bar',
  data: {
    labels: colorKeys,
    datasets:[{
      label:'Frecuencia Absoluta',
      data: colorKeys.map(c=>colorCounts[c]||0),
      backgroundColor: colorKeys.map(c=>COLORS[c].bg),
      borderColor: colorKeys.map(c=>COLORS[c].border),
      borderWidth: 2,
      borderRadius: 4,
    }]
  },
  options:{
    responsive:true, maintainAspectRatio:false,
    plugins:{ legend:{display:false} },
    scales:{
      y:{ beginAtZero:true, grid:{color:'rgba(255,255,255,.05)'}, ticks:{stepSize:1} },
      x:{ grid:{display:false} }
    }
  }
});

// ── PIE COLOR ──
new Chart(document.getElementById('pieChart'), {
  type: 'doughnut',
  data: {
    labels: colorKeys,
    datasets:[{
      data: colorKeys.map(c=>colorCounts[c]||0),
      backgroundColor: colorKeys.map(c=>COLORS[c].bg),
      borderColor: colorKeys.map(c=>COLORS[c].border),
      borderWidth: 2,
      hoverOffset: 10,
    }]
  },
  options:{
    responsive:true, maintainAspectRatio:false,
    cutout:'55%',
    plugins:{ legend:{ position:'bottom', labels:{ padding:16, usePointStyle:true } } }
  }
});

// ── BAR VELOCITY ──
new Chart(document.getElementById('barVelChart'), {
  type: 'bar',
  data: {
    labels: intervals.map(i=>i.label),
    datasets:[{
      label:'Frecuencia Absoluta',
      data: intervals.map(i=>i.fi),
      backgroundColor:'rgba(124,58,237,.6)',
      borderColor:'#7c3aed',
      borderWidth: 2,
      borderRadius: 4,
    }]
  },
  options:{
    responsive:true, maintainAspectRatio:false,
    plugins:{ legend:{display:false} },
    scales:{
      y:{ beginAtZero:true, grid:{color:'rgba(255,255,255,.05)'}, ticks:{stepSize:1} },
      x:{ grid:{display:false} }
    }
  }
});

// ── OJIVA ──
new Chart(document.getElementById('acumChart'), {
  type: 'line',
  data: {
    labels: acumRows.map(r=>r.hi),
    datasets:[{
      label:'Frecuencia Acumulada',
      data: acumRows.map(r=>r.Fi),
      borderColor:'#06d6a0',
      backgroundColor:'rgba(6,214,160,.1)',
      fill:true, tension:.4, pointRadius:5,
      pointBackgroundColor:'#06d6a0',
    }]
  },
  options:{
    responsive:true, maintainAspectRatio:false,
    plugins:{ legend:{display:false} },
    scales:{
      y:{ beginAtZero:true, max:n, grid:{color:'rgba(255,255,255,.05)'} },
      x:{ grid:{color:'rgba(255,255,255,.03)'} }
    }
  }
});

// ── POLÍGONO ──
const midpoints = intervals.map(i=>(i.lo+i.hi)/2);
new Chart(document.getElementById('polyChart'), {
  type: 'line',
  data: {
    labels: midpoints,
    datasets:[{
      label:'Frecuencia Absoluta',
      data: intervals.map(i=>i.fi),
      borderColor:'#f72585',
      backgroundColor:'rgba(247,37,133,.1)',
      fill:true, tension:.3, pointRadius:6,
      pointBackgroundColor:'#f72585',
      borderWidth:2,
    }]
  },
  options:{
    responsive:true, maintainAspectRatio:false,
    plugins:{ legend:{display:false} },
    scales:{
      y:{ beginAtZero:true, grid:{color:'rgba(255,255,255,.05)'}, ticks:{stepSize:1} },
      x:{
        title:{ display:true, text:'Punto medio del intervalo (km/h)', color:'#7070a0' },
        grid:{color:'rgba(255,255,255,.03)'}
      }
    }
  }
});

// ── HISTOGRAMA PESO ──
const pesoIntervals = [6,7,8,9,10,11,12,13];
const pesoHist = pesoIntervals.slice(0,-1).map((lo,i)=>{
  const hi=pesoIntervals[i+1];
  return { label:`[${lo}-${hi})`, fi: pesos.filter(p=>p>=lo&&p<hi).length };
});
new Chart(document.getElementById('pesoChart'), {
  type: 'bar',
  data: {
    labels: pesoHist.map(p=>p.label),
    datasets:[{
      label:'Frecuencia',
      data: pesoHist.map(p=>p.fi),
      backgroundColor:'rgba(124,58,237,.65)',
      borderColor:'#7c3aed',
      borderWidth:2, borderRadius:4,
    },{
      type:'line',
      label:'Media',
      data: pesoHist.map(()=>null).map((_,i,a)=>{
        const med = mean(pesos);
        // vertical line via dataset isn't ideal; skip
        return null;
      }),
    }]
  },
  options:{
    responsive:true, maintainAspectRatio:false,
    plugins:{
      legend:{display:false},
      annotation:{}
    },
    scales:{
      y:{ beginAtZero:true, grid:{color:'rgba(255,255,255,.05)'}, ticks:{stepSize:1} },
      x:{ grid:{display:false} }
    }
  }
});

// ── HISTOGRAMA ALTURA ──
const altIntervals = [50,55,60,65,70,75,80,85];
const altHist = altIntervals.slice(0,-1).map((lo,i)=>{
  const hi=altIntervals[i+1];
  return { label:`[${lo}-${hi})`, fi: alturas.filter(a=>a>=lo&&a<hi).length };
});
new Chart(document.getElementById('alturaChart'), {
  type: 'bar',
  data: {
    labels: altHist.map(a=>a.label),
    datasets:[{
      label:'Frecuencia',
      data: altHist.map(a=>a.fi),
      backgroundColor:'rgba(6,214,160,.65)',
      borderColor:'#06d6a0',
      borderWidth:2, borderRadius:4,
    }]
  },
  options:{
    responsive:true, maintainAspectRatio:false,
    plugins:{ legend:{display:false} },
    scales:{
      y:{ beginAtZero:true, grid:{color:'rgba(255,255,255,.05)'}, ticks:{stepSize:1} },
      x:{ grid:{display:false} }
    }
  }
});
</script>
</body>
</html>
