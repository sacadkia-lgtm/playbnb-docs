<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Play BNB — Announcement</title>
  <style>
    /* Basic reset */
    * { box-sizing: border-box; margin: 0; padding: 0; }

    /* Page background - pure black */
    html, body { height: 100%; }
    body {
      background: #000;
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      color: #fff;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      overflow: hidden; /* hide scrollbars caused by animated bubbles */
    }

    /* Bubble canvas container sits behind the content */
    .bubbles {
      position: fixed;
      inset: 0;
      pointer-events: none; /* allow clicks through */
      z-index: 0;
      overflow: hidden;
    }

    .bubble {
      position: absolute;
      border-radius: 50%;
      /* gold radial look */
      background: radial-gradient(circle at 35% 30%, #ffd84d 0%, #f1b700 30%, #b8860b 55%, rgba(184,134,11,0.10) 70%, transparent 75%);
      box-shadow: 0 0 10px rgba(255,215,0,0.12), 0 0 20px rgba(184,134,11,0.06) inset;
      opacity: 0.9;
      transform: translate3d(0,0,0) scale(1);
      will-change: transform, opacity;
      mix-blend-mode: screen;
    }

    /* Floating, slow animation */
    @keyframes floatY {
      0%   { transform: translateY(0) scale(1); opacity: 0.9; }
      50%  { transform: translateY(-18px) scale(1.06); opacity: 1; }
      100% { transform: translateY(0) scale(1); opacity: 0.9; }
    }
    @keyframes driftX {
      0%   { transform: translateX(0); }
      50%  { transform: translateX(8px); }
      100% { transform: translateX(0); }
    }

    /* Center card */
    .card-wrap {
      position: relative;
      z-index: 2; /* above bubbles */
      height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 24px;
    }

    .card {
      width: min(980px, 94%);
      background: #000; /* card black */
      border-radius: 14px;
      padding: 48px 36px;
      box-shadow: 0 10px 40px rgba(0,0,0,0.6), 0 0 0 1px rgba(255,215,0,0.03) inset;
      border: 1px solid rgba(255,215,0,0.02);
      text-align: center;
      color: #ffffff; /* white text */
      backdrop-filter: blur(6px);
    }

    /* Big headline */
    .headline {
      font-size: clamp(20px, 3.8vw, 40px);
      font-weight: 700;
      line-height: 1.05;
      color: #ffffff;
      margin-bottom: 20px;
      letter-spacing: -0.02em;
    }

    .subtext {
      font-size: clamp(13px, 1.8vw, 16px);
      color: rgba(255,255,255,0.90);
      margin-top: 8px;
      max-width: 860px;
      margin-left: auto;
      margin-right: auto;
    }

    /* small footer note */
    .note {
      margin-top: 22px;
      font-size: 13px;
      color: rgba(255,255,255,0.75);
    }

    /* Respect reduced motion preferences */
    @media (prefers-reduced-motion: reduce) {
      .bubble { animation: none !important; transition: none !important; }
    }

    /* Responsive spacing */
    @media (max-width: 520px) {
      .card { padding: 28px 18px; }
      .headline { font-size: 22px; }
    }
  </style>
</head>
<body>
  <!-- animated gold bubbles -->
  <div class="bubbles" id="bubbles"></div>

  <!-- main centered card -->
  <div class="card-wrap">
    <main class="card" role="main" aria-labelledby="headline">
      <h1 id="headline" class="headline">
        Only users who follow our X and Telegram channels will be eligible to win the airdrop.
      </h1>

      <p class="subtext">
        Winners who follow our official X and Telegram pages will receive rewards distributed through the Gaming and Airdrop sections. Make sure to follow both channels to increase your eligibility.
      </p>

      <p class="note">
        Follow our channels for announcements and rules. This is the official eligibility requirement for upcoming airdrops and in-game rewards.
      </p>
    </main>
  </div>

  <script>
    // Create a small number of animated gold bubbles, scattered and subtle.
    (function createBubbles() {
      const container = document.getElementById('bubbles');
      const COUNT = 18; // "very few" as requested
      const vw = Math.max(document.documentElement.clientWidth || 0, window.innerWidth || 0);
      const vh = Math.max(document.documentElement.clientHeight || 0, window.innerHeight || 0);

      for (let i = 0; i < COUNT; i++) {
        const b = document.createElement('div');
        b.className = 'bubble';

        // random size between 8 and 36 px (small and subtle)
        const size = Math.floor(Math.random() * 28) + 8;
        b.style.width = size + 'px';
        b.style.height = size + 'px';

        // random position but avoid centering directly over the card: bias to edges
        const margin = 6; // percent margin from edges
        const left = Math.random() * (100 - margin*2) + margin;
        const top = Math.random() * (100 - margin*2) + margin;

        b.style.left = left + 'vw';
        b.style.top = top + 'vh';

        // subtle staggered animation duration and delay
        const dur = 6 + Math.random() * 8; // 6s - 14s
        const delay = Math.random() * 6;   // 0 - 6s
        b.style.animation = `floatY ${dur}s ease-in-out ${delay}s infinite alternate, driftX ${Math.max(6, dur/1.2)}s ease-in-out ${delay/2}s infinite alternate`;

        // varied opacity and blur for depth
        b.style.opacity = (0.6 + Math.random() * 0.35).toFixed(2);
        // small transform offset for variety
        const initialScale = 0.85 + Math.random() * 0.35;
        b.style.transform = `scale(${initialScale})`;

        container.appendChild(b);
      }

      // Reposition bubbles on resize to keep them roughly in view
      let resizeTimer;
      window.addEventListener('resize', function() {
        clearTimeout(resizeTimer);
        resizeTimer = setTimeout(() => {
          // For simplicity just slightly nudge positions instead of full regen
          const items = container.children;
          for (let i = 0; i < items.length; i++) {
            const el = items[i];
            const left = parseFloat(el.style.left || '50').toFixed(2);
            const top = parseFloat(el.style.top || '50').toFixed(2);
            // small random tweak
            const nx = Math.min(96, Math.max(4, parseFloat(left) + (Math.random()*6-3)));
            const ny = Math.min(96, Math.max(4, parseFloat(top) + (Math.random()*6-3)));
            el.style.left = nx + 'vw';
            el.style.top = ny + 'vh';
          }
        }, 200);
      }, { passive: true });
    })();
  </script>
</body>
</html>
