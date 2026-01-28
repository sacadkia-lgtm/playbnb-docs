<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Play BNB — Tokenomics & Distribution</title>

  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg: #f7f9fc;
      --card: #fff;
      --muted: #6b7280;
      --accent: #6b46ff;
      --accent-2: #7f9cf5;
      --success: #10b981;
      --danger: #ef4444;
      --glass: rgba(255,255,255,0.75);
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial;
      background:linear-gradient(180deg,#f8fafc 0%,var(--bg) 100%);
      color:#0f172a;
      padding:28px;
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      line-height:1.4;
    }

    .wrapper{
      max-width:1100px;
      margin:0 auto;
    }

    .card{
      background:var(--card);
      border-radius:12px;
      padding:20px;
      box-shadow:0 12px 30px rgba(2,6,23,0.06);
      display:flex;
      gap:20px;
      align-items:flex-start;
      flex-wrap:wrap;
    }

    .left{
      flex:1 1 420px;
      min-width:320px;
    }

    .right{
      flex:1 1 420px;
      min-width:320px;
    }

    h1{margin:0 0 6px 0; font-size:20px}
    p.lead{margin:0 0 14px 0; color:var(--muted)}

    .chart-wrap{
      background:linear-gradient(180deg, rgba(107,70,255,0.06), rgba(127,156,245,0.02));
      padding:14px;
      border-radius:10px;
      display:flex;
      align-items:center;
      justify-content:center;
    }

    canvas{max-width:360px; width:100%; height:320px}

    .stats{
      display:flex;
      gap:14px;
      margin-top:12px;
      flex-wrap:wrap;
    }

    .stat{
      background:var(--glass);
      border-radius:8px;
      padding:10px 12px;
      min-width:170px;
      border:1px solid rgba(2,6,23,0.04);
    }
    .stat .label{font-size:13px;color:var(--muted)}
    .stat .value{font-weight:700;font-size:18px;margin-top:6px}

    .legend{
      margin-top:14px;
      display:grid;
      grid-template-columns:1fr 1fr;
      gap:10px;
    }
    .legend-item{
      display:flex;
      gap:10px;
      align-items:center;
      padding:8px;
      border-radius:8px;
      border:1px solid rgba(2,6,23,0.04);
      background:#fff;
    }
    .dot{
      width:14px;height:14px;border-radius:4px;
    }
    .muted{color:var(--muted);font-size:13px}

    .controls{
      display:flex;
      gap:8px;
      margin-top:12px;
      align-items:center;
      flex-wrap:wrap;
    }
    input.contract{
      padding:8px 10px;
      border-radius:8px;
      border:1px solid rgba(2,6,23,0.06);
      min-width:220px;
      font-family:inherit;
    }
    button.btn{
      background:var(--accent);
      color:white;
      border:none;
      padding:9px 12px;
      border-radius:8px;
      cursor:pointer;
      font-weight:600;
    }
    button.secondary{
      background:#eef2ff;
      color:#3730a3;
      border:none;
      padding:8px 12px;
      border-radius:8px;
      cursor:pointer;
      font-weight:600;
    }

    .content-section{
      margin-top:16px;
      padding-top:8px;
      border-top:1px dashed rgba(2,6,23,0.04);
    }
    h2{font-size:16px;margin:8px 0}
    ul{margin:8px 0 0 18px;color:var(--muted)}

    .footer-note{margin-top:12px;color:var(--muted);font-size:13px}

    @media (max-width:900px){
      .card{padding:14px}
      canvas{height:280px}
    }
  </style>
</head>
<body>
  <div class="wrapper">
    <div class="card" role="main">
      <div class="left">
        <h1>Play BNB — Tokenomics & Distribution</h1>
        <p class="lead">Play BNB is a token built on the BNB Chain (BEP-20). Below is the planned distribution, key supply numbers, and feature summaries for presale, airdrops, mining/rewards, and Web3 gaming incentives.</p>

        <div class="chart-wrap" aria-hidden="false">
          <canvas id="donutChart" aria-label="Token distribution chart" role="img"></canvas>
        </div>

        <div class="stats" aria-hidden="false">
          <div class="stat" title="Circulating supply">
            <div class="label">Circulating Supply</div>
            <div id="circulating" class="value">20,000,000,000</div>
          </div>
          <div class="stat" title="Total supply">
            <div class="label">Total Supply</div>
            <div id="total" class="value">100,000,000,000</div>
          </div>
        </div>

        <div class="legend" id="legend"></div>

        <div class="controls">
          <input id="contract" class="contract" placeholder="Contract address (BEP-20) — paste here" />
          <button id="copyBtn" class="btn">Copy Address</button>
          <button id="downloadBtn" class="secondary">Download CSV</button>
        </div>

        <div class="footer-note">Note: Percentages and labels are editable in the file. If you have a real contract address or vesting schedule, add them for transparency.</div>
      </div>

      <div class="right">
        <h2>Distribution Summary</h2>
        <p class="muted">The chart shows a sample distribution used for Play BNB. Change values as needed.</p>

        <div style="margin-top:8px">
          <strong>Default allocation (example)</strong>
          <ul>
            <li><strong>30%</strong> Reserve / Liquidity / Team — long-term reserves and liquidity pool funding.</li>
            <li><strong>30%</strong> Listing & Partnerships — tokens for exchange listings, market making, partnerships.</li>
            <li><strong>20%</strong> Presale — early access sale to raise funds and bootstrap liquidity.</li>
            <li><strong>10%</strong> Treasury — project treasury for operations, ecosystem grants.</li>
            <li><strong>10%</strong> Airdrop — community airdrops to boot network effects.</li>
          </ul>
        </div>

        <div class="content-section" aria-labelledby="presale">
          <h2 id="presale">Presale</h2>
          <p class="muted">Presale participants will be able to purchase Play BNB at early prices. Presale allocation is typically subject to KYC (optional), allocated tiers, and may include cliff/vesting schedules for some participants.</p>
        </div>

        <div class="content-section" aria-labelledby="airdrop">
          <h2 id="airdrop">Airdrop</h2>
          <p class="muted">Airdrops are used to reward early adopters and grow the community. Airdrop recipients may be required to complete tasks like following social accounts, joining the community, or holding a minimum balance.</p>
        </div>

        <div class="content-section" aria-labelledby="mining">
          <h2 id="mining">Mining / Rewards for Active Users</h2>
          <p class="muted">Play BNB supports on-chain "mining" style rewards for active participants — players, stakers, content creators, and community contributors. Mining rewards can be distributed from Reserve or a tailored reward pool and are typically subject to rate limits and anti-abuse measures.</p>
        </div>

        <div class="content-section" aria-labelledby="gaming">
          <h2 id="gaming">Web3 Gaming & Incentives</h2>
          <p class="muted">Play BNB is designed for Web3 gaming use-cases: in-game token rewards, leaderboard prizes, NFT ownership, and play-to-earn mechanics. Game participation will grant points and token rewards, driving retention and engagement.</p>
        </div>
      </div>
    </div>
  </div>

  <!-- Chart.js CDN -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
  <script>
    // Token supply numbers (edit here if needed)
    const TOTAL_SUPPLY = 100_000_000_000;        // 100 billion
    const CIRCULATING_SUPPLY = 20_000_000_000;   // 20 billion

    // Distribution configuration (percentages must sum to 100)
    const distribution = [
      { key: 'reserve', label: 'Reserve / Liquidity / Team', pct: 30, color: '#6b46ff' },
      { key: 'listing', label: 'Listing & Partnerships', pct: 30, color: '#5b21b6' },
      { key: 'presale', label: 'Presale', pct: 20, color: '#7f9cf5' },
      { key: 'treasury', label: 'Treasury', pct: 10, color: '#c7d2fe' },
      { key: 'airdrop', label: 'Airdrop', pct: 10, color: '#94a3b8' }
    ];

    // DOM elements
    document.getElementById('total').textContent = TOTAL_SUPPLY.toLocaleString();
    document.getElementById('circulating').textContent = CIRCULATING_SUPPLY.toLocaleString();

    // Prepare chart data
    const labels = distribution.map(d => d.label);
    const dataValues = distribution.map(d => d.pct);
    const colors = distribution.map(d => d.color);

    // Create chart
    const ctx = document.getElementById('donutChart').getContext('2d');
    const donut = new Chart(ctx, {
      type: 'doughnut',
      data: {
        labels,
        datasets: [{
          data: dataValues,
          backgroundColor: colors,
          borderColor: '#fff',
          borderWidth: 2,
          hoverOffset: 12
        }]
      },
      options: {
        cutout: '60%',
        plugins: {
          legend: { display: false },
          tooltip: {
            callbacks: {
              label: function(context) {
                const pct = context.parsed;
                const label = context.label || '';
                const absolute = Math.round((pct / 100) * TOTAL_SUPPLY);
                return label + ' — ' + pct + '% (' + absolute.toLocaleString() + ' tokens)';
              }
            }
          }
        },
        responsive: true,
        maintainAspectRatio: false
      }
    });

    // Build legend
    const legend = document.getElementById('legend');
    distribution.forEach(d => {
      const el = document.createElement('div');
      el.className = 'legend-item';
      el.innerHTML = '<div class="dot" style="background:' + d.color + '"></div>' +
                     '<div style="flex:1"><div style="font-weight:700">' + d.label + '</div>' +
                     '<div class="muted">' + d.pct + '% — ' + Math.round((d.pct/100)*TOTAL_SUPPLY).toLocaleString() + ' tokens</div></div>';
      legend.appendChild(el);
    });

    // Copy contract address
    document.getElementById('copyBtn').addEventListener('click', function(){
      const input = document.getElementById('contract');
      const text = input.value.trim();
      if(!text){
        this.textContent = 'No address';
        setTimeout(()=> this.textContent = 'Copy Address', 1400);
        return;
      }
      navigator.clipboard.writeText(text).then(() => {
        const prev = this.textContent;
        this.textContent = 'Copied';
        setTimeout(()=> this.textContent = prev, 1400);
      }).catch(()=> {
        this.textContent = 'Copy failed';
        setTimeout(()=> this.textContent = 'Copy Address', 1400);
      });
    });

    // Download CSV
    document.getElementById('downloadBtn').addEventListener('click', function(){
      const rows = [['key','label','percentage','tokens']];
      distribution.forEach(d => rows.push([
        d.key,
        d.label,
        d.pct,
        Math.round((d.pct/100) * TOTAL_SUPPLY)
      ]));
      const csv = rows.map(r => r.map(c => '"' + String(c).replace(/"/g,'""') + '"').join(',')).join('\\n');
      const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
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
