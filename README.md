<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Team 23586 | MS Robotics Resources</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,400;0,500;0,600;0,700&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #0a0e17;
      --surface: #111827;
      --surface-hover: #1a2332;
      --border: #1e293b;
      --border-light: #334155;
      --text: #e2e8f0;
      --text-muted: #94a3b8;
      --text-dim: #64748b;
      --accent: #06d6a0;
      --accent-dim: rgba(6, 214, 160, 0.12);
      --accent-glow: rgba(6, 214, 160, 0.25);
      --accent-secondary: #38bdf8;
      --accent-secondary-dim: rgba(56, 189, 248, 0.1);
      --gradient-start: #06d6a0;
      --gradient-end: #38bdf8;
    }

    * { margin: 0; padding: 0; box-sizing: border-box; }

    body {
      font-family: 'DM Sans', sans-serif;
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* ── Ambient background ── */
    .bg-glow {
      position: fixed;
      top: -200px;
      right: -200px;
      width: 600px;
      height: 600px;
      background: radial-gradient(circle, var(--accent-glow) 0%, transparent 70%);
      pointer-events: none;
      z-index: 0;
      animation: drift 20s ease-in-out infinite alternate;
    }
    .bg-glow-2 {
      position: fixed;
      bottom: -300px;
      left: -200px;
      width: 700px;
      height: 700px;
      background: radial-gradient(circle, rgba(56, 189, 248, 0.08) 0%, transparent 70%);
      pointer-events: none;
      z-index: 0;
      animation: drift 25s ease-in-out infinite alternate-reverse;
    }
    @keyframes drift {
      0% { transform: translate(0, 0); }
      100% { transform: translate(-60px, 40px); }
    }

    /* ── Layout ── */
    .container {
      max-width: 860px;
      margin: 0 auto;
      padding: 0 24px;
      position: relative;
      z-index: 1;
    }

    /* ── Top nav bar ── */
    nav {
      padding: 20px 0;
      display: flex;
      align-items: center;
      justify-content: space-between;
      border-bottom: 1px solid var(--border);
    }
    .nav-brand {
      font-family: 'Space Mono', monospace;
      font-size: 14px;
      color: var(--accent);
      letter-spacing: 0.5px;
    }
    .nav-links {
      display: flex;
      gap: 28px;
      list-style: none;
    }
    .nav-links a {
      font-size: 14px;
      font-weight: 500;
      color: var(--text-muted);
      text-decoration: none;
      transition: color 0.2s;
      position: relative;
    }
    .nav-links a:hover { color: var(--text); }
    .nav-links a.active { color: var(--accent); }
    .nav-links a.active::after {
      content: '';
      position: absolute;
      bottom: -4px;
      left: 0;
      right: 0;
      height: 2px;
      background: var(--accent);
      border-radius: 1px;
    }

    /* ── Hero ── */
    .hero {
      padding: 80px 0 60px;
    }
    .team-tag {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 6px 14px;
      border-radius: 100px;
      border: 1px solid var(--border-light);
      background: var(--surface);
      font-family: 'Space Mono', monospace;
      font-size: 13px;
      color: var(--accent);
      margin-bottom: 24px;
      animation: fadeUp 0.6s ease-out both;
    }
    .team-tag .dot {
      width: 6px;
      height: 6px;
      border-radius: 50%;
      background: var(--accent);
      animation: pulse 2s ease-in-out infinite;
    }
    @keyframes pulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.3; }
    }
    .hero h1 {
      font-size: clamp(36px, 6vw, 56px);
      font-weight: 700;
      line-height: 1.1;
      letter-spacing: -1.5px;
      margin-bottom: 20px;
      animation: fadeUp 0.6s ease-out 0.1s both;
    }
    .hero h1 .gradient {
      background: linear-gradient(135deg, var(--gradient-start), var(--gradient-end));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }
    .hero p {
      font-size: 18px;
      color: var(--text-muted);
      line-height: 1.7;
      max-width: 560px;
      animation: fadeUp 0.6s ease-out 0.2s both;
    }

    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(16px); }
      to { opacity: 1; transform: translateY(0); }
    }

    /* ── Cards grid ── */
    .section-label {
      font-family: 'Space Mono', monospace;
      font-size: 12px;
      letter-spacing: 2px;
      text-transform: uppercase;
      color: var(--text-dim);
      margin-bottom: 20px;
    }
    .cards {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 16px;
      margin-bottom: 60px;
    }
    .card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 14px;
      padding: 28px 24px;
      text-decoration: none;
      color: var(--text);
      transition: all 0.25s ease;
      position: relative;
      overflow: hidden;
      animation: fadeUp 0.5s ease-out both;
    }
    .card:nth-child(1) { animation-delay: 0.3s; }
    .card:nth-child(2) { animation-delay: 0.4s; }
    .card:nth-child(3) { animation-delay: 0.5s; }
    .card:nth-child(4) { animation-delay: 0.6s; }

    .card::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      height: 2px;
      background: linear-gradient(90deg, var(--gradient-start), var(--gradient-end));
      opacity: 0;
      transition: opacity 0.25s;
    }
    .card:hover {
      border-color: var(--border-light);
      background: var(--surface-hover);
      transform: translateY(-2px);
    }
    .card:hover::before { opacity: 1; }
    .card-icon {
      width: 40px;
      height: 40px;
      border-radius: 10px;
      background: var(--accent-dim);
      display: flex;
      align-items: center;
      justify-content: center;
      margin-bottom: 16px;
      font-size: 20px;
    }
    .card-icon.blue {
      background: var(--accent-secondary-dim);
    }
    .card h3 {
      font-size: 16px;
      font-weight: 600;
      margin-bottom: 8px;
      letter-spacing: -0.3px;
    }
    .card p {
      font-size: 14px;
      color: var(--text-muted);
      line-height: 1.55;
    }
    .card .arrow {
      position: absolute;
      top: 28px;
      right: 24px;
      color: var(--text-dim);
      transition: all 0.25s;
      font-size: 18px;
    }
    .card:hover .arrow {
      color: var(--accent);
      transform: translateX(3px);
    }

    /* ── Info sections ── */
    .info-block {
      padding: 40px 0;
      border-top: 1px solid var(--border);
      animation: fadeUp 0.5s ease-out 0.7s both;
    }
    .info-block h2 {
      font-size: 22px;
      font-weight: 700;
      margin-bottom: 12px;
      letter-spacing: -0.5px;
    }
    .info-block p {
      font-size: 15px;
      color: var(--text-muted);
      line-height: 1.7;
      max-width: 600px;
    }
    .info-block a {
      color: var(--accent);
      text-decoration: none;
      border-bottom: 1px solid transparent;
      transition: border-color 0.2s;
    }
    .info-block a:hover {
      border-bottom-color: var(--accent);
    }

    /* ── Contributors ── */
    .contributors {
      display: flex;
      flex-wrap: wrap;
      gap: 12px;
      margin-top: 20px;
    }
    .contributor {
      display: inline-flex;
      align-items: center;
      gap: 10px;
      padding: 8px 16px;
      border-radius: 100px;
      border: 1px solid var(--border);
      background: var(--surface);
      font-size: 14px;
      color: var(--text-muted);
      transition: border-color 0.2s;
    }
    .contributor:hover {
      border-color: var(--border-light);
    }
    .contributor .avatar {
      width: 24px;
      height: 24px;
      border-radius: 50%;
      background: linear-gradient(135deg, var(--gradient-start), var(--gradient-end));
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 11px;
      font-weight: 700;
      color: var(--bg);
    }
    .contributor.lead {
      border-color: var(--accent);
      color: var(--text);
    }
    .contributor.lead .avatar {
      box-shadow: 0 0 8px var(--accent-glow);
    }

    /* ── Footer ── */
    footer {
      padding: 40px 0;
      border-top: 1px solid var(--border);
      margin-top: 20px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 16px;
    }
    footer p {
      font-size: 13px;
      color: var(--text-dim);
    }
    .footer-links {
      display: flex;
      gap: 20px;
    }
    .footer-links a {
      font-size: 13px;
      color: var(--text-dim);
      text-decoration: none;
      transition: color 0.2s;
    }
    .footer-links a:hover { color: var(--text); }

    /* ── Mobile ── */
    @media (max-width: 600px) {
      .hero { padding: 48px 0 40px; }
      nav { flex-direction: column; align-items: flex-start; gap: 16px; }
      .cards { grid-template-columns: 1fr; }
      footer { flex-direction: column; align-items: flex-start; }
    }
  </style>
</head>
<body>
  <div class="bg-glow"></div>
  <div class="bg-glow-2"></div>

  <div class="container">
    <nav>
      <div class="nav-brand">23586://resources</div>
      <ul class="nav-links">
        <li><a href="https://potatzz.github.io/23586/" class="active">Home</a></li>
        <li><a href="https://potatzz.github.io/23586/table_of_contents.html">Contents</a></li>
      </ul>
    </nav>

    <section class="hero">
      <div class="team-tag">
        <span class="dot"></span>
        FTC Team 23586
      </div>
      <h1>Middle School<br><span class="gradient">Robotics Resources</span></h1>
      <p>Your hub for applications, links, guides, and everything you need. Built by the team, for the team.</p>
    </section>

    <div class="section-label">Quick Links</div>
    <div class="cards">
      <a href="https://potatzz.github.io/23586/table_of_contents.html" class="card">
        <div class="card-icon">📋</div>
        <span class="arrow">→</span>
        <h3>Table of Contents</h3>
        <p>Browse all pages and resources organized in one place.</p>
      </a>
      <a href="mailto:cstinson29@priorypanther.com" class="card">
        <div class="card-icon blue">✉️</div>
        <span class="arrow">→</span>
        <h3>Report an Issue</h3>
        <p>Something broken or missing? Let us know and we'll fix it.</p>
      </a>
      <!-- Add more cards here as your site grows -->
    </div>

    <div class="info-block">
      <h2>Navigation</h2>
      <p>Every page has links at the bottom to get you home. You can also click the brand mark in the top left to jump back to this page at any time. The <a href="https://potatzz.github.io/23586/table_of_contents.html">Table of Contents</a> has a full index of everything on the site.</p>
    </div>

    <div class="info-block">
      <h2>Issues &amp; Feedback</h2>
      <p>If you run into any problems — broken links, missing pages, or anything else — reach out to <a href="mailto:cstinson29@priorypanther.com">cstinson29@priorypanther.com</a> or DM <strong style="color:var(--text);">@potatz</strong> on Discord. You can also find us around school.</p>
    </div>

    <div class="info-block">
      <h2>Contributors</h2>
      <p>This site is built and maintained by team members.</p>
      <div class="contributors">
        <div class="contributor lead">
          <span class="avatar">CS</span>
          Cowboy Stinson '29
        </div>
        <div class="contributor">
          <span class="avatar">LW</span>
          Lucas Walker '30
        </div>
        <div class="contributor">
          <span class="avatar">NS</span>
          Nik Shah '29
        </div>
        <div class="contributor">
          <span class="avatar">AS</span>
          Alexander Sudhof '29
        </div>
      </div>
    </div>

    <footer>
      <p>&copy; 2026 Team 23586 · Woodside Priory School</p>
      <div class="footer-links">
        <a href="https://potatzz.github.io/23586/">Home</a>
        <a href="https://potatzz.github.io/23586/table_of_contents.html">Contents</a>
        <a href="mailto:cstinson29@priorypanther.com">Contact</a>
      </div>
    </footer>
  </div>
</body>
</html>
