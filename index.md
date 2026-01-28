<!doctype html>
<html lang="fa" dir="rtl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Play BNB — توزیع و اطلاعات توکن</title>
  <link href="https://fonts.googleapis.com/css2?family=Vazirmatn:wght@300;400;700&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg:#fafafa;
      --card:#ffffff;
      --muted:#666;
      --accent:#6b46ff;
      --accent-2:#7f9cf5;
      --success:#2f855a;
      --danger:#e53e3e;
    }
    html,body{height:100%}
    body{
      margin:0;
      font-family:"Vazirmatn",system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial;
      background:linear-gradient(180deg,#fbfbfe 0%,var(--bg) 100%);
      color:#111827;
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      padding:24px;
      box-sizing:border-box;
    }
    .container{
      max-width:980px;
      margin:0 auto;
      background:var(--card);
      border-radius:12px;
      box-shadow:0 6px 24px rgba(16,24,40,0.06);
      padding:24px;
      display:flex;
      gap:24px;
      align-items:center;
      flex-wrap:wrap;
    }
    .chart-panel{
      flex:1 1 360px;
      min-width:320px;
      display:flex;
      gap:16px;
      align-items:center;
      justify-content:center;
      flex-direction:column;
    }
    canvas{max-width:360px; width:100%; height:auto}
    .info-panel{
      flex:1 1 360px;
      min-width:320px;
      padding:12px 8px;
    }
    h1{margin:0 0 8px 0; font-size:20px}
    p.lead{margin:0 0 16px 0; color:var(--muted)}
    .supply{
      display:flex;
      gap:18px;
      align-items:center;
      margin:18px 0;
      flex-wrap:wrap;
    }
    .supply .item{
      background:#f8f9fb;
      border-radius:8px;
      padding:12px 14px;
      min-width:180px;
      text-align:center;
    }
    .supply .item .label{font-size:13px;color:var(--muted)}
    .supply .item .value{font-weight:700;font-size:18px;margin-top:6px}
    .legend{
      display:grid;
      grid-template-columns:1fr 1fr;
      gap:10px;
      margin-top:8px;
    }
    .leg{
      display:flex;
      gap:10px;
      align-items:center;
      padding:8px;
      border-radius:8px;
      background:#fff;
      border:1px solid #f0f2f7;
    }
    .dot{
      width:14px;height:14px;border-radius:4px;
    }
    .muted{color:var(--muted);font-size:13px}
    .contract{
      margin-top:16px;
      display:flex;
      gap:8px;
      align-items:center;
      flex-wrap:wrap;
    }
    input.copy{
      padding:8px 10px;
      border-radius:8px;
      border:1px solid #e6edf8;
      font-family:inherit;
      min-width:220px;
      direction:ltr;
      text-align:left;
    }
    button.btn{
      background:var(--accent);
      color:white;
      border:none;
      padding:8px 12px;
      border-radius:8px;
      cursor:pointer;
      font-weight:600;
    }
    .note{margin-top:12px;color:var(--muted);font-size:13px}
    @media (max-width:720px){
      .container{padding:16px}
      .chart-panel, .info-panel{min-width:100%}
    }
  </style>
</head>
<body>
  <div class="container" role="main">
    <div class="chart-panel" aria-label="نمودار توزیع توکن">
      <h1>Play BNB — توزیع توکن</h1>
      <p class="lead">نمایش تصویری تقسیم‌بندی و اعداد عرضه (قابل ویرایش).</p>
      <canvas id="tokenChart" aria-hidden="false"></canvas>
      <div class="note">نمودار Donut نشان‌دهندهٔ درصد هر بخش از کل عرضه است.</div>
    </div>

    <div class="info-panel">
      <div class="supply">
        <div class="item">
          <div class="label">عرضه در گردش</div>
          <div class="value">20,000,000,000</div>
        </div>
        <div class="item">
          <div class="label">کل عرضه</div>
          <div class="value">100,000,000,000</div>
        </div>
      </div>

      <div class="legend" id="legend">
        <!-- لیست بخش‌ها با رنگ و درصد در اینجا قرار می‌گیرد -->
      </div>

      <div class="contract">
        <input id="contractInput" class="copy" value="آدرس قرارداد (در صورت وجود این‌جا قرار دهید)" />
        <button class="btn" id="copyBtn">کپی</button>
        <button class="btn" id="downloadBtn" style="background:var(--success)">دانلود CSV</button>
      </div>

      <div class="note">
        توضیحات: Reserve = ذخیره/نقدینگی/تیم؛ Listing = توکن‌های منظورشده برای لیستینگ/صرافی؛ Presale = فروش اولیه؛ Treasury = صندوق؛ Airdrop = توزیع رایگان.
      </div>
    </div>
  </div>

  <!-- Chart.js CDN -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
  <script>
    // داده‌های اولیه (طبق تصویر). اگر می‌خواهید درصدها تغییر کنند، این آرایه را ویرایش کنید.
    const distribution = [
      { label: 'Reserve / Liquidity / Team', value: 30, color: '#6b46ff' },
      { label: 'Listing', value: 30, color: '#5b21b6' },
      { label: 'Presale', value: 20, color: '#7f9cf5' },
      { label: 'Treasury', value: 10, color: '#cbd5e1' },
      { label: 'Airdrop', value: 10, color: '#a3a3a3' }
    ];

    const ctx = document.getElementById('tokenChart').getContext('2d');
    const data = {
      labels: distribution.map(d => d.label),
      datasets: [{
        data: distribution.map(d => d.value),
        backgroundColor: distribution.map(d => d.color),
        hoverOffset: 8,
        borderColor: '#fff',
        borderWidth: 2
      }]
    };

    const config = {
      type: 'doughnut',
      data,
      options: {
        cutout: '62%',
        plugins: {
          legend: { display: false },
          tooltip: {
            callbacks: {
              label: context => {
                const v = context.parsed;
                const total = context.dataset.data.reduce((s,a)=>s+a,0);
                return context.label + ': ' + v + '% (' + ((v/100)*100).toLocaleString() + ')';
              }
            }
          }
        },
        responsive: true,
        maintainAspectRatio: false
      }
    };

    const myChart = new Chart(ctx, config);

    // ایجاد لیبل/قائمه دلخواه سمت راست/چپ (RTL)
    const legendEl = document.getElementById('legend');
    distribution.forEach(d=>{
      const li = document.createElement('div');
      li.className = 'leg';
      li.innerHTML = '<div class="dot" style="background:' + d.color + '"></div>' +
                     '<div style="flex:1"><div style="font-weight:700">' + d.label + '</div><div class="muted">' + d.value + '%</div></div>';
      legendEl.appendChild(li);
    });

    // کپی آدرس قرارداد
    document.getElementById('copyBtn').addEventListener('click', ()=>{
      const input = document.getElementById('contractInput');
      navigator.clipboard.writeText(input.value).then(()=>{
        const old = document.getElementById('copyBtn').textContent;
        document.getElementById('copyBtn').textContent = 'کپی شد';
        setTimeout(()=> document.getElementById('copyBtn').textContent = old, 1500);
      });
    });

    // تولید CSV ساده از توزیع و دانلود
    document.getElementById('downloadBtn').addEventListener('click', ()=>{
      const rows = [['label','percentage']];
      distribution.forEach(d => rows.push([d.label, d.value]));
      const csv = rows.map(r => r.map(c => '"' + String(c).replace(/"/g,'""') + '"').join(',')).join('\\n');
      const blob = new Blob([csv], {type: 'text/csv;charset=utf-8;'});
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'playbnb_distribution.csv';
      document.body.appendChild(a);
      a.click();
      a.remove();
      URL.revokeObjectURL(url);
    });
  </script>
</body>
</html>
