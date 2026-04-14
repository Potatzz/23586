<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Key CAD Concepts | Team 23586</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,400;0,500;0,600;0,700&family=Space+Mono:wght@400;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
  <style>
/* ══════════════════════════════════════════════════
   Team 23586 — Shared Styles
   ══════════════════════════════════════════════════ */


:root {
  --bg: #0a0e17;
  --surface: #111827;
  --surface-hover: #1a2332;
  --surface-code: #0d1117;
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
  --yellow: #fbbf24;
  --yellow-dim: rgba(251, 191, 36, 0.12);
  --red: #f87171;
  --red-dim: rgba(248, 113, 113, 0.12);
  --purple: #a78bfa;
  --purple-dim: rgba(167, 139, 250, 0.12);
  --orange: #fb923c;
  --orange-dim: rgba(251, 146, 60, 0.12);
}

* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  font-family: 'DM Sans', sans-serif;
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  overflow-x: hidden;
  line-height: 1.7;
}

/* ── Ambient background ── */
.bg-glow {
  position: fixed; top: -200px; right: -200px;
  width: 600px; height: 600px;
  background: radial-gradient(circle, var(--accent-glow) 0%, transparent 70%);
  pointer-events: none; z-index: 0;
  animation: drift 20s ease-in-out infinite alternate;
}
.bg-glow-2 {
  position: fixed; bottom: -300px; left: -200px;
  width: 700px; height: 700px;
  background: radial-gradient(circle, rgba(56, 189, 248, 0.08) 0%, transparent 70%);
  pointer-events: none; z-index: 0;
  animation: drift 25s ease-in-out infinite alternate-reverse;
}
@keyframes drift {
  0% { transform: translate(0, 0); }
  100% { transform: translate(-60px, 40px); }
}
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(16px); }
  to { opacity: 1; transform: translateY(0); }
}

/* ── Layout ── */
.container {
  max-width: 860px;
  margin: 0 auto;
  padding: 0 24px;
  position: relative;
  z-index: 1;
}

/* ── Nav ── */
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
  text-decoration: none;
  transition: opacity 0.2s;
}
.nav-brand:hover { opacity: 0.8; }
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
  bottom: -4px; left: 0; right: 0;
  height: 2px;
  background: var(--accent);
  border-radius: 1px;
}

/* ── Page header ── */
.page-header {
  padding: 60px 0 40px;
  animation: fadeUp 0.5s ease-out both;
}
.page-header h1 {
  font-size: clamp(30px, 5vw, 42px);
  font-weight: 700;
  letter-spacing: -1px;
  margin-bottom: 10px;
}
.page-header h1 .gradient {
  background: linear-gradient(135deg, var(--gradient-start), var(--gradient-end));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}
.page-header p {
  font-size: 16px;
  color: var(--text-muted);
  line-height: 1.6;
  max-width: 620px;
}

/* ── Badges ── */
.badge {
  font-family: 'Space Mono', monospace;
  font-size: 10px;
  letter-spacing: 0.5px;
  text-transform: uppercase;
  padding: 3px 10px;
  border-radius: 4px;
  font-weight: 700;
  display: inline-block;
  vertical-align: middle;
}
.badge.wip { background: var(--yellow-dim); color: var(--yellow); }
.badge.windows { background: var(--accent-secondary-dim); color: var(--accent-secondary); }
.badge.caution { background: var(--orange-dim); color: var(--orange); }

/* ── Content article ── */
.content {
  animation: fadeUp 0.5s ease-out 0.15s both;
}
.content h2 {
  font-size: 24px;
  font-weight: 700;
  letter-spacing: -0.5px;
  margin: 48px 0 16px;
  padding-top: 32px;
  border-top: 1px solid var(--border);
}
.content h2:first-child {
  margin-top: 0;
  padding-top: 0;
  border-top: none;
}
.content h3 {
  font-size: 18px;
  font-weight: 600;
  letter-spacing: -0.3px;
  margin: 32px 0 12px;
  color: var(--text);
}
.content p {
  font-size: 15px;
  color: var(--text-muted);
  line-height: 1.75;
  margin-bottom: 16px;
  max-width: 680px;
}
.content a {
  color: var(--accent);
  text-decoration: none;
  border-bottom: 1px solid transparent;
  transition: border-color 0.2s;
}
.content a:hover { border-bottom-color: var(--accent); }
.content strong { color: var(--text); font-weight: 600; }
.content ul, .content ol {
  margin: 12px 0 20px 24px;
  color: var(--text-muted);
  font-size: 15px;
}
.content li {
  margin-bottom: 8px;
  line-height: 1.6;
}
.content li strong { color: var(--text); }

/* ── Code blocks ── */
.content pre {
  background: var(--surface-code);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 20px 24px;
  margin: 16px 0 24px;
  overflow-x: auto;
  font-family: 'JetBrains Mono', monospace;
  font-size: 13px;
  line-height: 1.7;
  color: #c9d1d9;
  -webkit-overflow-scrolling: touch;
}
.content code {
  font-family: 'JetBrains Mono', monospace;
  font-size: 13px;
  background: var(--surface);
  border: 1px solid var(--border);
  padding: 2px 7px;
  border-radius: 5px;
  color: var(--accent);
}
.content pre code {
  background: none;
  border: none;
  padding: 0;
  border-radius: 0;
  color: inherit;
  font-size: inherit;
}

/* ── Images ── */
.content img {
  max-width: 100%;
  border-radius: 10px;
  border: 1px solid var(--border);
  margin: 16px 0 24px;
  display: block;
}
.content .img-caption {
  font-size: 13px;
  color: var(--text-dim);
  margin-top: -16px;
  margin-bottom: 24px;
  font-style: italic;
}

/* ── Callout boxes ── */
.callout {
  background: var(--surface);
  border: 1px solid var(--border);
  border-left: 3px solid var(--accent);
  border-radius: 8px;
  padding: 16px 20px;
  margin: 20px 0 24px;
  font-size: 14px;
  color: var(--text-muted);
  line-height: 1.6;
}
.callout.warning {
  border-left-color: var(--yellow);
}
.callout.info {
  border-left-color: var(--accent-secondary);
}
.callout strong { color: var(--text); }

/* ── Challenge boxes ── */
.challenges {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 24px 28px;
  margin: 32px 0;
}
.challenges h3 {
  font-size: 16px;
  font-weight: 700;
  margin: 0 0 12px;
  color: var(--accent);
}
.challenges ul {
  margin: 0 0 0 20px;
  color: var(--text-muted);
  font-size: 14px;
}
.challenges li { margin-bottom: 6px; }

/* ── Footer ── */
footer {
  padding: 40px 0;
  border-top: 1px solid var(--border);
  margin-top: 48px;
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
  .page-header { padding: 40px 0 28px; }
  nav { flex-direction: column; align-items: flex-start; gap: 16px; }
  .content pre { padding: 14px 16px; font-size: 12px; }
  footer { flex-direction: column; align-items: flex-start; }
}

  </style>
</head>
<body>
  <div class="bg-glow"></div>
  <div class="bg-glow-2"></div>
  <div class="container">
    <nav>
      <a href="https://potatzz.github.io/23586/" class="nav-brand">23586://resources</a>
      <ul class="nav-links">
        <li><a href="https://potatzz.github.io/23586/">Home</a></li>
        <li><a href="https://potatzz.github.io/23586/table_of_contents.html">Contents</a></li>
      </ul>
    </nav>

    <section class="page-header">
      <h1><span class="gradient">Key CAD Concepts</span></h1>
      <p>Guides and information on CAD for FTC. We mainly use Onshape, a free online CAD package with FTC support. <span class="badge wip">WIP</span></p>
    </section>

    <article class="content">

      <div class="callout info">
        <strong>Onshape:</strong> We mainly use <a href="https://www.onshape.com/en/" target="_blank" rel="noopener">Onshape</a>, a free online CAD package. If you have an account, you should already be added to our MS Robotics group which gives you access to hundreds of GoBilda and REV Robotics models. If you don't have an account, you can use your school email to make a free one.
      </div>

      <h2>Your First Part</h2>
      <p>Ready to make your first part! First click <strong>"Create"</strong> then on the drop down menu click <strong>"Document"</strong>.</p>
      <p>Now you're in your first Document/Project! It might seem a bit overwhelming, but we're going to break it down step by step.</p>

      <h3>The Viewport</h3>
      <img src="https://potatzz.github.io/23586/images/onshape-layout.png" alt="Onshape layout showing the viewport">
      <p>This is the <strong>viewport</strong>. You'll see 3 planes that give reference to the rest of the project. The cube in the corner allows you to quickly change the plane you're viewing. You can also use right click or two-finger click to change camera angles.</p>

      <h3>The Feature List</h3>
      <p>This displays all of your features in your project. It's split into two parts:</p>
      <ul>
        <li><strong>Features</strong> — things that modify parts; generally what you use in Onshape</li>
        <li><strong>Parts</strong> — their own objects; used to tell if things are connected, export things, or put them into an assembly</li>
      </ul>

      <h3>The Toolbar</h3>
      <p>Where you access all your tools. The search bar on the right allows you to search for specific tools. There are two different toolbars: the <strong>sketch tools</strong> and the <strong>3D tools</strong>. You'll need to be fluent with both to model effectively.</p>

      <h2>Making a Symmetrical Box</h2>
      <p>Now let's make your first part — a symmetrical box.</p>
      <ol>
        <li>Click <strong>"Sketch"</strong> on the left side of the 3D toolbox</li>
        <li>Click the <strong>top plane</strong>, and adjust your view to be looking at it head-on. This swaps your toolbox into the sketch toolbox.</li>
        <li>To make something 3D, you must start with something 2D first! Find the <strong>rectangle tool</strong> in the sketch toolbox and draw a rectangle of any shape or size. Then press <code>Esc</code>.</li>
      </ol>

      <h3>Smart Dimension</h3>
      <p>Press <code>D</code> to start a <strong>smart dimension</strong>. This lets you make lines specific lengths — you'll use this tool a lot!</p>
      <ol>
        <li>Your cursor should become a plus sign. Click on one of the sides of the rectangle.</li>
        <li>Drag your cursor off the line — a black arch should follow your cursor.</li>
        <li>Click again to place the dimension, and type in the length you want.</li>
        <li>Press <strong>Enter</strong> and repeat for a perpendicular side (same length since it's a square).</li>
      </ol>

      <div class="callout info">
        <strong>Why only 2 sides?</strong> When you use the rectangle tool, opposite sides are forced to be the same length, so you only need to dimension 2 sides. If you do all 4, Onshape will give an error — even if you set both to the same value. Think of it like two people screaming in your ear at the same time. Even if they're saying the same thing, it's hard to understand.
      </div>

      <p><span class="badge wip">WIP</span> — More steps coming soon.</p>

    </article>

    <footer>
      <p>&copy; 2026 Team 23586 · Woodside Priory School</p>
      <div class="footer-links">
        <a href="https://potatzz.github.io/23586/">Home</a>
        <a href="https://potatzz.github.io/23586/table_of_contents.html">Contents</a>
        <a href="/cdn-cgi/l/email-protection#20435354494e534f4e1219605052494f525950414e544845520e434f4d">Contact</a>
