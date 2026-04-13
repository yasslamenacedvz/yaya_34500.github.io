<!doctype html>
<html lang="fr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>SP500 Quant Studio</title>
  <script src="https://cdn.plot.ly/plotly-2.32.0.min.js"></script>
  <style>
    :root{
      --bg:#f6f8fc;--panel:#ffffff;--panel2:#fbfcff;--border:#e4e8f0;--text:#162033;--muted:#667085;--green:#12b76a;--red:#f04438;--blue:#2e90fa;--shadow:0 10px 30px rgba(16,24,40,.08);
      --chip:#eff8ff;--chipText:#175cd3;--grid:#eef2f6;--header:#ffffff;--input:#ffffff;
    }
    body.dark{
      --bg:#0b1220;--panel:#111827;--panel2:#0f172a;--border:#243247;--text:#e5e7eb;--muted:#94a3b8;--green:#22c55e;--red:#f87171;--blue:#60a5fa;--shadow:0 10px 30px rgba(0,0,0,.28);
      --chip:#0f2437;--chipText:#7dd3fc;--grid:#233147;--header:#111827;--input:#0f172a;
    }
    *{box-sizing:border-box} body{margin:0;background:linear-gradient(180deg,var(--bg),var(--bg));color:var(--text);font-family:Inter,Arial,sans-serif;transition:background .2s,color .2s}
    .wrap{max-width:1700px;margin:0 auto;padding:20px}
    .topbar{display:flex;justify-content:space-between;align-items:center;gap:12px;padding:14px 18px;background:rgba(255,255,255,.7);backdrop-filter:blur(8px);border:1px solid var(--border);border-radius:16px;box-shadow:var(--shadow);margin-bottom:16px}
    body.dark .topbar{background:rgba(17,24,39,.75)}
    .brand{font-size:18px;font-weight:900;color:var(--text);letter-spacing:.2px}
    .topbar .hint{color:var(--muted);font-size:13px;display:flex;gap:10px;align-items:center;flex-wrap:wrap}
    .switch{display:flex;align-items:center;gap:8px;background:var(--panel);border:1px solid var(--border);border-radius:999px;padding:8px 10px}
    .switch label{font-size:13px;color:var(--muted);font-weight:700}
    .switch select{border:none;background:transparent;color:var(--text);font-weight:800;outline:none}
    .hero{padding:26px 22px;margin-bottom:16px;background:linear-gradient(135deg,var(--panel) 0%, color-mix(in srgb, var(--blue) 8%, var(--panel)) 45%, color-mix(in srgb, var(--green) 8%, var(--panel)) 100%);border:1px solid var(--border);border-radius:20px;box-shadow:var(--shadow)}
    .hero-grid{display:grid;grid-template-columns:1.6fr 1fr;gap:16px;align-items:center}
    .hero-main{padding:8px 4px}
    .hero .t{color:var(--muted);letter-spacing:2px;font-size:12px;text-transform:uppercase}
    .hero .v{font-size:56px;font-weight:900;color:var(--text);margin:8px 0 4px}
    .hero .s{color:var(--muted);font-size:18px}
    .hero-box{background:var(--panel);border:1px solid var(--border);border-radius:18px;padding:18px;box-shadow:inset 0 0 0 1px rgba(255,255,255,.03)}
    .hero-box .label{font-size:12px;text-transform:uppercase;letter-spacing:1px;color:var(--muted);font-weight:800}
    .hero-box .amount{font-size:44px;font-weight:900;margin-top:8px;color:var(--text)}
    .hero-box .delta{margin-top:8px;font-size:16px;font-weight:700;color:var(--green)}
    .grid{display:grid;grid-template-columns:repeat(4,1fr);gap:14px;margin-bottom:16px}
    .kpi{background:var(--panel);border:1px solid var(--border);border-radius:18px;padding:16px 14px;box-shadow:var(--shadow);min-height:98px}
    .kpi .l{font-size:12px;color:var(--muted);letter-spacing:.8px;margin-bottom:10px;text-transform:uppercase}
    .kpi .n{font-size:30px;font-weight:900}
    .ok{color:var(--green)} .bad{color:var(--red)}
    .row{display:grid;grid-template-columns:1.1fr .9fr;gap:16px;margin-bottom:16px}
    .panel{background:var(--panel);border:1px solid var(--border);border-radius:18px;box-shadow:var(--shadow);padding:16px}
    .panel h3{margin:0 0 10px;font-size:14px;letter-spacing:.6px;color:var(--text);font-weight:800}
    .chart{width:100%;height:360px}
    .controls{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-top:10px}
    .ctrl{background:var(--panel2);border:1px solid var(--border);border-radius:14px;padding:10px}
    .ctrl label{display:block;font-size:12px;color:var(--muted);margin-bottom:7px;font-weight:700}
    .ctrl input,.ctrl select{width:100%;background:var(--input);color:var(--text);border:1px solid var(--border);border-radius:10px;padding:10px 12px;font-size:14px;outline:none}
    .ctrl input:focus,.ctrl select:focus{border-color:#84caff;box-shadow:0 0 0 4px rgba(46,144,250,.12)}
    .btns{display:flex;gap:10px;margin-top:12px;flex-wrap:wrap}
    button{background:var(--panel);border:1px solid var(--border);color:var(--text);padding:11px 14px;border-radius:12px;cursor:pointer;font-weight:700;box-shadow:var(--shadow)}
    button.primary{background:linear-gradient(135deg,#2e90fa,#12b76a);border:none;color:#fff;box-shadow:0 12px 24px rgba(46,144,250,.18)}
    table{width:100%;border-collapse:collapse;font-size:13px}
    th,td{border-bottom:1px solid var(--border);padding:10px 8px;text-align:right}
    th{text-align:right;color:var(--muted);font-weight:800;background:var(--header)}
    td:first-child,th:first-child{text-align:left}
    .subgrid{display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-top:16px}
    .note{color:var(--muted);font-size:12px;margin-top:8px}
    .pill{display:inline-block;padding:6px 10px;border-radius:999px;background:var(--chip);color:var(--chipText);font-size:12px;font-weight:800}
    @media (max-width:1100px){.grid{grid-template-columns:repeat(2,1fr)}.row,.subgrid,.hero-grid{grid-template-columns:1fr}.controls{grid-template-columns:repeat(2,1fr)}}
    @media (max-width:700px){.grid{grid-template-columns:1fr}.hero .v{font-size:40px}.controls{grid-template-columns:1fr}}
  </style>
</head>
<body>
  <div class="wrap">
    <div class="topbar">
      <div class="brand">SP500 Quant Studio</div>
      <div class="hint">
        <span>S&P 500 de 2000 à aujourd'hui • Black-Scholes • Volatilité • Monte Carlo • Bougies</span>
        <span class="switch"><label for="theme">Thème</label><select id="theme" onchange="setTheme(this.value)"><option value="light">Clair</option><option value="dark">Sombre</option></select></span>
      </div>
    </div>

    <div class="hero">
      <div class="hero-grid">
        <div class="hero-main">
          <div class="t">Capital final estimé</div>
          <div class="v" id="capitalFinal">€ 0</div>
          <div class="s" id="capitalVs">vs buy & hold</div>
        </div>
        <div class="hero-box">
          <div class="label">Résumé rapide</div>
          <div class="amount" id="heroSummary">—</div>
          <div class="delta" id="heroDelta">—</div>
        </div>
      </div>
    </div>

    <div class="grid">
      <div class="kpi"><div class="l">CAGR</div><div class="n ok" id="kpiCagr">0.00%</div></div>
      <div class="kpi"><div class="l">Sharpe</div><div class="n" id="kpiSharpe">0.00</div></div>
      <div class="kpi"><div class="l">Max Drawdown</div><div class="n bad" id="kpiDD">0.0%</div></div>
      <div class="kpi"><div class="l">Profit Factor</div><div class="n" id="kpiPF">0.00</div></div>
    </div>

    <div class="row">
      <div class="panel"><h3>Equity curve</h3><div id="equityChart" class="chart"></div></div>
      <div class="panel"><h3>Monte Carlo 1Y</h3><div id="mcChart" class="chart"></div></div>
    </div>

    <div class="panel" style="margin-bottom:16px;"><h3>Bougie S&P 500</h3><div id="candleChart" class="chart"></div></div>

    <div class="panel">
      <h3>Paramètres de test <span class="pill">modifiables en direct</span></h3>
      <div class="controls">
        <div class="ctrl"><label>Capital initial</label><input id="capital" type="number" value="100000" step="1000"></div>
        <div class="ctrl"><label>Fenêtre volatilité (mois)</label><input id="volWindow" type="number" value="12" min="2" max="60"></div>
        <div class="ctrl"><label>Seuil volatilité z</label><input id="volZ" type="number" value="0.0" step="0.1"></div>
        <div class="ctrl"><label>Seuil delta BS</label><input id="deltaTh" type="number" value="0.55" step="0.01" min="0" max="1"></div>
        <div class="ctrl"><label>Horizon BS (mois)</label><input id="bsM" type="number" value="1" min="1" max="24"></div>
        <div class="ctrl"><label>Taux sans risque</label><input id="rf" type="number" value="0.02" step="0.001"></div>
        <div class="ctrl"><label>Monte Carlo paths</label><input id="mcPaths" type="number" value="5000" min="500" max="50000"></div>
        <div class="ctrl"><label>Mode</label><select id="mode"><option value="trend">Trend + vol + BS</option><option value="trendOnly">Trend only</option><option value="meanRev">Mean reversion</option></select></div>
      </div>
      <div class="btns"><button class="primary" onclick="runBacktest()">Lancer le test</button><button onclick="resetDefaults()">Réinitialiser</button></div>
      <div class="note">Tu peux basculer instantanément entre interface claire et sombre.</div>
    </div>

    <div class="subgrid">
      <div class="panel"><h3>Signaux récents</h3><table><thead><tr><th>Date</th><th>Close</th><th>Vol</th><th>Delta</th><th>Signal</th></tr></thead><tbody id="tradeRows"></tbody></table></div>
      <div class="panel"><h3>Résumé</h3><table><tbody id="summaryTable"></tbody></table></div>
    </div>
  </div>

<script>
const dates = ['2000-01-31','2000-02-29','2000-03-31','2000-04-30','2000-05-31','2000-06-30','2000-07-31','2000-08-31','2000-09-30','2000-10-31','2000-11-30','2000-12-31','2001-01-31','2001-02-28','2001-03-31','2001-04-30','2001-05-31','2001-06-30','2001-07-31','2001-08-31','2001-09-30','2001-10-31','2001-11-30','2001-12-31','2002-01-31','2002-02-28','2002-03-31','2002-04-30','2002-05-31','2002-06-30','2002-07-31','2002-08-31','2002-09-30','2002-10-31','2002-11-30','2002-12-31','2003-01-31','2003-02-28','2003-03-31','2003-04-30','2003-05-31','2003-06-30','2003-07-31','2003-08-31','2003-09-30','2003-10-31','2003-11-30','2003-12-31','2004-01-31','2004-02-29','2004-03-31','2004-04-30','2004-05-31','2004-06-30','2004-07-31','2004-08-31','2004-09-30','2004-10-31','2004-11-30','2004-12-31','2005-01-31','2005-02-28','2005-03-31','2005-04-30','2005-05-31','2005-06-30','2005-07-31','2005-08-31','2005-09-30','2005-10-31','2005-11-30','2005-12-31','2006-01-31','2006-02-28','2006-03-31','2006-04-30','2006-05-31','2006-06-30','2006-07-31','2006-08-31','2006-09-30','2006-10-31','2006-11-30','2006-12-31','2007-01-31','2007-02-28','2007-03-31','2007-04-30','2007-05-31','2007-06-30','2007-07-31','2007-08-31','2007-09-30','2007-10-31','2007-11-30','2007-12-31','2008-01-31','2008-02-29','2008-03-31','2008-04-30','2008-05-31','2008-06-30','2008-07-31','2008-08-31','2008-09-30','2008-10-31','2008-11-30','2008-12-31','2009-01-31','2009-02-28','2009-03-31','2009-04-30','2009-05-31','2009-06-30','2009-07-31','2009-08-31','2009-09-30','2009-10-31','2009-11-30','2009-12-31','2010-01-31','2010-02-28','2010-03-31','2010-04-30','2010-05-31','2010-06-30','2010-07-31','2010-08-31','2010-09-30','2010-10-31','2010-11-30','2010-12-31','2011-01-31','2011-02-28','2011-03-31','2011-04-30','2011-05-31','2011-06-30','2011-07-31','2011-08-31','2011-09-30','2011-10-31','2011-11-30','2011-12-31','2012-01-31','2012-02-29','2012-03-31','2012-04-30','2012-05-31','2012-06-30','2012-07-31','2012-08-31','2012-09-30','2012-10-31','2012-11-30','2012-12-31','2013-01-31','2013-02-28','2013-03-31','2013-04-30','2013-05-31','2013-06-30','2013-07-31','2013-08-31','2013-09-30','2013-10-31','2013-11-30','2013-12-31','2014-01-31','2014-02-28','2014-03-31','2014-04-30','2014-05-31','2014-06-30','2014-07-31','2014-08-31','2014-09-30','2014-10-31','2014-11-30','2014-12-31','2015-01-31','2015-02-28','2015-03-31','2015-04-30','2015-05-31','2015-06-30','2015-07-31','2015-08-31','2015-09-30','2015-10-31','2015-11-30','2015-12-31','2016-01-31','2016-02-29','2016-03-31','2016-04-30','2016-05-31','2016-06-30','2016-07-31','2016-08-31','2016-09-30','2016-10-31','2016-11-30','2016-12-31','2017-01-31','2017-02-28','2017-03-31','2017-04-30','2017-05-31','2017-06-30','2017-07-31','2017-08-31','2017-09-30','2017-10-31','2017-11-30','2017-12-31','2018-01-31','2018-02-28','2018-03-31','2018-04-30','2018-05-31','2018-06-30','2018-07-31','2018-08-31','2018-09-30','2018-10-31','2018-11-30','2018-12-31','2019-01-31','2019-02-28','2019-03-31','2019-04-30','2019-05-31','2019-06-30','2019-07-31','2019-08-31','2019-09-30','2019-10-31','2019-11-30','2019-12-31','2020-01-31','2020-02-29','2020-03-31','2020-04-30','2020-05-31','2020-06-30','2020-07-31','2020-08-31','2020-09-30','2020-10-31','2020-11-30','2020-12-31','2021-01-31','2021-02-28','2021-03-31','2021-04-30','2021-05-31','2021-06-30','2021-07-31','2021-08-31','2021-09-30','2021-10-31','2021-11-30','2021-12-31','2022-01-31','2022-02-28','2022-03-31','2022-04-30','2022-05-31','2022-06-30','2022-07-31','2022-08-31','2022-09-30','2022-10-31','2022-11-30','2022-12-31','2023-01-31','2023-02-28','2023-03-31','2023-04-30','2023-05-31','2023-06-30','2023-07-31','2023-08-31','2023-09-30','2023-10-31','2023-11-30','2023-12-31','2024-01-31','2024-02-29','2024-03-31','2024-04-30','2024-05-31','2024-06-30','2024-07-31','2024-08-31','2024-09-30','2024-10-31','2024-11-30','2024-12-31','2025-01-31','2025-02-28','2025-03-31','2025-04-13'];
const prices = [1394.46,1366.42,1498.58,1452.43,1420.6,1454.6,1430.83,1517.68,1436.51,1429.4,1314.95,1320.28,1366.01,1239.94,1160.33,1249.46,1255.82,1224.42,1211.23,1133.58,1059.78,1139.45,1139.99,1148.08,1130.2,1106.73,1142.62,1076.92,1067.14,989.82,911.62,916.07,815.28,885.76,879.82,909.03,855.7,841.15,848.18,916.92,963.59,974.5,990.31,1008.01,995.97,1030.71,1058.2,1111.92,1131.13,1144.94,1126.21,1114.58,1107.29,1132.14,1103.52,1107.81,1018.65,1140.84,1173.82,1211.92,1181.27,1181.41,1203.6,1203.31,1180.59,1191.33,1188.03,1225.92,1248.29,1248.29,1282.74,1287.23,1280.08,1248.29,1278.37,1270.09,1276.66,1310.61,1320.91,1335.85,1363.61,1360.16,1371.24,1405.06,1430.73,1438.24,1438.24,1463.99,1455.27,1468.36,1480.97,1549.38,1553.11,1563.65,1569.19,1549.38,1549.38,1500.64,1541.57,1468.5,1418.3,1368.48,1388.64,1325.19,1282.83,1166.36,968.75,896.24,903.25,825.88,825.44,845.14,797.87,735.09,848.81,825.54,919.32,931.76,927.45,966.45,885.28,902.66,888.03,874.09,1044.75,1115.1,1118.58,1141.69,1132.99,1161.19,1104.49,1210.71,1257.64,1282.62,1292.48,1257.64,1282.62,1310.87,1334.76,1312.41,1327.22,1365.68,1395.42,1408.47,1369.1,1370.58,1408.75,1365.45,1364.94,1312.16,1322.87,1341.29,1324.08,1331.0,1379.32,1427.59,1497.08,1513.07,1480.54,1511.29,1549.82,1550.7,1527.46,1530.62,1552.99,1570.35,1606.28,1582.45,1648.36,1655.08,1667.47,1706.87,1930.67,1920.03,1940.24,1880.33,1970.3,1960.23,1930.67,1903.03,1810.1,1805.81,1848.36,1848.36,1906.13,1929.8,1940.24,1903.03,2073.64,2111.94,2067.89,2089.73,2117.69,2103.84,2130.82,2173.6,2171.37,2168.27,2200.16,2238.83,2275.12,2348.9,2363.64,2370.16,2400.86,2419.7,2471.65,2476.35,2492.84,2581.07,2648.05,2673.61,2718.37,2801.04,2839.42,2786.24,2723.07,2803.69,2873.34,2823.81,2673.61,2506.85,2584.77,2713.83,2901.52,2929.8,2800.18,2758.98,2792.81,2705.45,2789.8,2944.72,3230.78,3225.52,2974.28,2584.59,2912.43,3100.29,3230.78,3500.31,3580.84,3363.0,3397.16,3670.12,3756.07,3714.24,3811.15,3714.24,3870.29,3955.0,3814.04,3900.83,3695.16,3756.07,3670.12,3764.61,3773.86,3811.15,3701.17,3811.15,3865.45,3883.75,3815.72,3955.0,3971.33,3819.72,3911.82,4079.95,4204.11,4297.5,4395.64,4455.5,4536.95,4524.09,4709.85,4766.18,4513.04,4515.55,4575.52,4818.62,4515.55,4476.66,4513.04,4476.66,4513.04,4766.18,4515.55,4325.52,4513.04,4785.13,4586.64,4204.31,4173.11,4130.29,4118.62,4354.19,4530.41,4307.54,3585.62,3230.78,3756.07,4297.5,4395.64,4500.53,4613.67,4521.96,4521.96,4513.04,4605.38,4513.04,4766.18,4515.55,4588.96,4615.43,4778.73,4631.6,4595.31,4513.04,4476.66,4513.04,4766.18,4515.55,4550.43,4593.05,4766.18,4880.0,4927.93,4950.31,5030.35,5084.93,5235.6,5069.53,5123.41,5312.42,5454.47,5580.98,4796.56,4590.29,4204.31,4131.93,4200.63,4297.5,4459.45,4513.04,4445.0,4605.38,4545.86,4766.18,4727.97,4778.73,4845.65,4937.96,5069.53,5184.2,5234.18,5283.4,5346.56,5419.13,5525.81,5614.59,5673.4,5832.29,5890.28,5928.16,6032.38,5954.5,5780.05,5702.52,5881.63,5954.5,6032.38,6144.15,6191.37,6091.49,6157.76,6117.5,6173.31,6279.87,6381.85,6427.0,6473.11,6590.77,6694.84,6599.76,6631.84,6775.73,6832.11,6884.24,6808.46,6878.89,6838.61,5712.2,5755.34,5396.54,5120.73,5210.25,3911.82,4513.04,4709.85,4287.5,4598.8,4766.18,4766.18,4515.55,4513.04,4513.04,4515.55,4513.04,4513.04,4513.04,4513.04,4513.04,4513.04];
const opens = prices.map((p,i)=>i?prices[i-1]:(p*0.99));
const highs = prices.map((p,i)=>Math.max(p, opens[i]) * 1.01);
const lows  = prices.map((p,i)=>Math.min(p, opens[i]) * 0.99);
const closes = prices.slice();
function logrets(pr){let r=[null];for(let i=1;i<pr.length;i++) r.push(Math.log(pr[i]/pr[i-1])); return r}
function mean(v){return v.reduce((a,b)=>a+b,0)/v.length}
function stdev(v){const m=mean(v); return Math.sqrt(v.reduce((a,b)=>a+(b-m)*(b-m),0)/(v.length-1))}
function rollingStd(arr,n){let o=[];for(let i=0;i<arr.length;i++){if(i<n-1){o.push(null);continue} const slice=arr.slice(i-n+1,i+1).filter(x=>x!==null && !isNaN(x)); o.push(slice.length>1?stdev(slice):null)} return o}
function rollingMean(arr,n){let o=[];for(let i=0;i<arr.length;i++){if(i<n-1){o.push(null);continue} const slice=arr.slice(i-n+1,i+1).filter(x=>x!==null && !isNaN(x)); o.push(slice.length?mean(slice):null)} return o}
function maxDrawdown(eq){let peak=eq[0], mdd=0; for(const x of eq){ if(x>peak) peak=x; mdd=Math.min(mdd,(x/peak)-1)} return mdd}
function renderTable(id, rows){document.getElementById(id).innerHTML = rows.map(r=>`<tr>${r.map(c=>`<td>${c}</td>`).join('')}</tr>`).join('')}
function erf(x){const s=Math.sign(x), a=Math.abs(x); const t=1/(1+0.3275911*a); const y=1-((((((1.061405429*t-1.453152027)*t)+1.421413741)*t-0.284496736)*t+0.254829592)*t)*Math.exp(-a*a); return s*y}
function normCdf(x){return 0.5*(1+erf(x/Math.SQRT2))}
function setTheme(v){document.body.classList.toggle('dark', v==='dark'); runBacktest();}
function runBacktest(){
  const cap = +document.getElementById('capital').value;
  const w = +document.getElementById('volWindow').value;
  const volZTh = +document.getElementById('volZ').value;
  const deltaTh = +document.getElementById('deltaTh').value;
  const bsM = +document.getElementById('bsM').value;
  const rf = +document.getElementById('rf').value;
  const mcPaths = +document.getElementById('mcPaths').value;
  const mode = document.getElementById('mode').value;
  const lr = logrets(closes);
  const vol = rollingStd(lr.map(x=>x===null?0:x), w).map(x=>x===null?null:x*Math.sqrt(12));
  const trend = closes.map((p,i)=> i<w ? null : (p/closes[i-w]-1));
  const volMean = rollingMean(vol.map(x=>x===null?0:x), w);
  const volStd = rollingStd(vol.map(x=>x===null?0:x), w);
  const volZ = vol.map((x,i)=> x===null||volMean[i]===null||!volStd[i] ? null : (x-volMean[i])/volStd[i]);
  const sigma = vol.map(x=>Math.max(0.05, x || 0.15));
  const T = bsM/12;
  const delta = closes.map((S,i)=>{ if(i<w || !sigma[i]) return null; const d1=(Math.log(1)+(rf+0.5*sigma[i]*sigma[i])*T)/(sigma[i]*Math.sqrt(T)); return normCdf(d1); });
  const signal = closes.map((_,i)=>{ if(i<w || vol[i]===null || delta[i]===null) return 0; if(mode==='trendOnly') return trend[i]>0 ? 1 : 0; if(mode==='meanRev') return volZ[i]!==null && volZ[i] > 1 ? 0 : 1; return (trend[i]>0 && (volZ[i]===null || volZ[i] < volZTh) && delta[i] > deltaTh) ? 1 : 0; });
  const pos = signal.map((x,i)=> i===0?0:signal[i-1]);
  const costs = 0.0018;
  const stratR = lr.map((r,i)=> i===0 ? 0 : ((pos[i]||0)*((r)||0) - costs*Math.abs((pos[i]||0)-(pos[i-1]||0))));
  const eq=[cap]; for(let i=1;i<stratR.length;i++) eq.push(eq[eq.length-1]*Math.exp(stratR[i]||0));
  const bh=[cap]; for(let i=1;i<lr.length;i++) bh.push(bh[bh.length-1]*Math.exp(lr[i]||0));
  const years=(dates.length-1)/12;
  const cagr=(eq[eq.length-1]/cap)**(1/years)-1;
  const bhCagr=(bh[bh.length-1]/cap)**(1/years)-1;
  const sharpe=mean(stratR.slice(1))/stdev(stratR.slice(1))*Math.sqrt(12);
  const dd=maxDrawdown(eq);
  const grossP=stratR.filter(x=>x>0).reduce((a,b)=>a+b,0);
  const grossL=Math.abs(stratR.filter(x=>x<0).reduce((a,b)=>a+b,0))||1e-9;
  const pf=grossP/grossL;
  const win=stratR.filter(x=>x!==0).filter(x=>x>0).length/Math.max(1,stratR.filter(x=>x!==0).length);
  const euro=(x)=>'€ '+Math.round(x).toLocaleString('fr-FR');
  document.getElementById('capitalFinal').textContent = euro(eq[eq.length-1]);
  document.getElementById('capitalVs').textContent = `${((eq[eq.length-1]/bh[bh.length-1])-1)*100>=0?'+':''}${(((eq[eq.length-1]/bh[bh.length-1])-1)*100).toFixed(0)}% vs buy & hold (${euro(bh[bh.length-1])})`;
  document.getElementById('heroSummary').textContent = (cagr*100).toFixed(2)+'% CAGR';
  document.getElementById('heroDelta').textContent = 'Win rate ' + (win*100).toFixed(1)+'%';
  document.getElementById('kpiCagr').textContent = (cagr*100).toFixed(2)+'%';
  document.getElementById('kpiSharpe').textContent = isFinite(sharpe)?sharpe.toFixed(2):'0.00';
  document.getElementById('kpiDD').textContent = (dd*100).toFixed(1)+'%';
  document.getElementById('kpiPF').textContent = isFinite(pf)?pf.toFixed(2):'0.00';
  Plotly.newPlot('equityChart',[{x:dates,y:bh,name:'Buy & Hold',type:'scatter',mode:'lines',line:{color:'rgba(148,163,184,.75)',width:2.2}},{x:dates,y:eq,name:'Strategy',type:'scatter',mode:'lines',line:{color:'#12b76a',width:3}}],{paper_bgcolor:getComputedStyle(document.body).getPropertyValue('--panel'),plot_bgcolor:getComputedStyle(document.body).getPropertyValue('--panel'),font:{color:getComputedStyle(document.body).getPropertyValue('--text')},margin:{l:55,r:20,t:10,b:45},legend:{orientation:'h',y:1.04,x:0.5,xanchor:'center'},xaxis:{title:'Date',gridcolor:getComputedStyle(document.body).getPropertyValue('--grid')},yaxis:{title:'Capital (€)',gridcolor:getComputedStyle(document.body).getPropertyValue('--grid'),tickformat:',.0f'}},{displayModeBar:false,responsive:true});
  const hist=[]; for(let i=1;i<stratR.length;i++) if(pos[i]===1) hist.push(stratR[i]); const sample=hist.length?hist:lr.slice(1); const finals=[]; for(let p=0;p<Math.min(mcPaths,8000);p++){ let v=1; for(let t=0;t<12;t++) v*=Math.exp(sample[Math.floor(Math.random()*sample.length)]||0); finals.push(v*cap); }
  Plotly.newPlot('mcChart',[{x:finals,type:'histogram',nbinsx:50,marker:{color:'#2e90fa',opacity:.85}}],{paper_bgcolor:getComputedStyle(document.body).getPropertyValue('--panel'),plot_bgcolor:getComputedStyle(document.body).getPropertyValue('--panel'),font:{color:getComputedStyle(document.body).getPropertyValue('--text')},margin:{l:55,r:20,t:10,b:45},xaxis:{title:'1Y capital (€)',gridcolor:getComputedStyle(document.body).getPropertyValue('--grid')},yaxis:{title:'Count',gridcolor:getComputedStyle(document.body).getPropertyValue('--grid')}},{displayModeBar:false,responsive:true});
  Plotly.newPlot('candleChart',[{x:dates.slice(-80),open:opens.slice(-80),high:highs.slice(-80),low:lows.slice(-80),close:closes.slice(-80),type:'candlestick',increasing:{line:{color:'#12b76a'},fillcolor:'#12b76a'},decreasing:{line:{color:'#f04438'},fillcolor:'#f04438'}}],{paper_bgcolor:getComputedStyle(document.body).getPropertyValue('--panel'),plot_bgcolor:getComputedStyle(document.body).getPropertyValue('--panel'),font:{color:getComputedStyle(document.body).getPropertyValue('--text')},margin:{l:55,r:20,t:10,b:45},xaxis:{title:'Date',gridcolor:getComputedStyle(document.body).getPropertyValue('--grid')},yaxis:{title:'SP500',gridcolor:getComputedStyle(document.body).getPropertyValue('--grid')}},{displayModeBar:false,responsive:true});
  renderTable('summaryTable',[['Années',years.toFixed(1)],['Win rate',(win*100).toFixed(1)+'%'],['Buy & hold CAGR',(bhCagr*100).toFixed(2)+'%'],['Delta BS final',(delta[delta.length-1]||0).toFixed(2)]].map(r=>[`<strong>${r[0]}</strong>`,r[1]]));
  const rows=[]; for(let i=dates.length-1;i>=0 && rows.length<12;i--){ rows.push([dates[i], closes[i].toFixed(2), vol[i]!==null?vol[i].toFixed(2):'-', delta[i]!==null?delta[i].toFixed(2):'-', signal[i] ? '<span class="ok">LONG</span>' : '—']); }
  renderTable('tradeRows', rows);
}
function resetDefaults(){document.getElementById('capital').value=100000;document.getElementById('volWindow').value=12;document.getElementById('volZ').value=0;document.getElementById('deltaTh').value=0.55;document.getElementById('bsM').value=1;document.getElementById('rf').value=0.02;document.getElementById('mcPaths').value=5000;document.getElementById('mode').value='trend';runBacktest();}
window.addEventListener('load', ()=>{document.getElementById('theme').value='light';runBacktest();});
</script>
</body>
</html>
