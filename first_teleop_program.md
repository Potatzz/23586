<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Making Your First TeleOp Program | Team 23586</title>
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
      <h1><span class="gradient">Making Your First TeleOp Program</span></h1>
      <p>Walk through making a simple program to move a motor via the joystick in Android Studio.</p>
    </section>

    <article class="content">

      <div class="callout warning">
        <strong>Prerequisites:</strong> Make sure you have Road Runner and Android Studio installed. If you don't, go <a href="https://potatzz.github.io/23586/code_setup.html">here</a> to install them.
      </div>

      <h2>Some Terminology and Structure First</h2>
      <p>Before we begin, let's go over some basic terminology. I highly suggest you also look at the <a href="https://potatzz.github.io/23586/codelibrary.html">Code Library</a> as well.</p>
      <p>The robot uses two pieces of hardware to interpret and run the code:</p>
      <ul>
        <li><strong>The Driver Station</strong> — allows the driver to run and get values from your code</li>
        <li><strong>The Control Hub</strong> — actually outputs the code and runs the motors</li>
      </ul>
      <p>When writing your code, there are a few methods that are required for the robot to run:</p>
      <ul>
        <li><code>init()</code> — runs <strong>once</strong> when you press "Init" on the Driver Station</li>
        <li><code>loop()</code> — runs <strong>50x a second</strong></li>
      </ul>
      <p>There are 3 other optional methods:</p>
      <ul>
        <li><code>init_loop()</code> — runs 50x/sec after you press "Init" and before you press "Start"</li>
        <li><code>start()</code> — runs once after you press "Start"</li>
        <li><code>stop()</code> — runs once after you press "Stop"</li>
      </ul>
      <p>Our code will only use the <code>init()</code> and <code>loop()</code> methods for now.</p>

      <h2>Start of Your Program</h2>
      <p>First, create a new Java Class in the <code>teamcode</code> folder. Name it <strong>"MyFirstTeleOpProgram"</strong>. Everything should start properly set up and your file should look something like this:</p>
<pre><code>package org.firstinspires.ftc.teamcode.IntoTheDeep.TeleOp;

public class MyFirstTeleOpProgram {

}</code></pre>
      <p>This program will be moving one motor. To start, create a new variable with the type <code>DcMotorEx</code> and name it <code>motor</code>. You should get an error and <code>DcMotorEx</code> should be highlighted red. It might suggest that you import a class — this is normal! If it suggests it, import it. A new line should appear at the top:</p>
<pre><code>import com.qualcomm.robotcore.hardware.DcMotorEx;</code></pre>
      <p>If you didn't get the suggestion, just copy and paste the import line above into your code.</p>

      <h2>Using <code>init()</code></h2>
      <p>Next, we're going to add one of our required methods. Inside of our class, create a new method with the header <code>public void init() {</code>. Inside, we'll allow the Driver Station to locate a specific motor from the control hub!</p>
      <p>Add the line:</p>
<pre><code>motor = hardwareMap.get(DcMotorEx.class, "testMotor");</code></pre>
      <p>This sets the <code>motor</code> variable to a specific motor name and type. The part we really care about is <code>"testMotor"</code> at the end. In the Driver Station, you'll assign a port on the Control Hub to a specific name. This name should match what you put here, so the Control Hub and Driver Station know which motor to move and which port it's plugged into.</p>
      <p>This line should give you another error on <code>hardwareMap.get</code> — import the class if asked! At this point your code should look like this:</p>
<pre><code>package org.firstinspires.ftc.teamcode.IntoTheDeep.TeleOp;

import static org.firstinspires.ftc.robotcore.external
    .BlocksOpModeCompanion.hardwareMap;

import com.qualcomm.robotcore.hardware.DcMotorEx;

public class MyFirstTeleOpProgram {
    DcMotorEx motor;

    public void init() {
        motor = hardwareMap.get(DcMotorEx.class, "testMotor");
    }
}</code></pre>

      <h2>Movement!</h2>
      <p>Finally, let's get the motor to move! Create a new method with the header <code>public void loop() {</code>.</p>
      <p>Inside the <code>loop()</code> method, we're going to create an <strong>if/else</strong> statement that reads the gamepad joysticks to move the motor. First write:</p>
<pre><code>if (gamepad1.left_stick_y > 0) {</code></pre>
      <p>This checks if the y value of the left joystick on the gamepad is greater than 0 — in other words, are you moving the joystick up? You should get an error on <code>gamepad1</code>, so import the class!</p>
      <p>Now add the line to actually move the motor:</p>
<pre><code>motor.setPower(1.0);</code></pre>

      <div class="callout info">
        <strong>Keep in mind:</strong> The <code>.setPower()</code> attribute requires a <em>double</em> to work, so provide a double and not an integer. Motors have a <strong>-1.0 to 1.0 range of power</strong> — anything higher or lower just gets clamped to -1.0 or 1.0.
      </div>

      <p>Now we have an issue — if you ran this code and moved the joystick, the motor would never stop! This could be dangerous. Add an <code>else</code> statement that sets the motor power to 0 when the joystick isn't pushed forward:</p>
<pre><code>else {
    motor.setPower(0.0);
}</code></pre>
      <p><strong>Notice how even zeros need a ".0" for doubles!</strong></p>
      <p>Here's the final code:</p>
<pre><code>package org.firstinspires.ftc.teamcode.IntoTheDeep.TeleOp;

import static org.firstinspires.ftc.robotcore.external
    .BlocksOpModeCompanion.gamepad1;
import static org.firstinspires.ftc.robotcore.external
    .BlocksOpModeCompanion.hardwareMap;

import com.qualcomm.robotcore.hardware.DcMotorEx;

public class MyFirstTeleOpProgram {
    DcMotorEx motor;

    public void init() {
        motor = hardwareMap.get(DcMotorEx.class, "motor");
    }

    public void loop() {
        if (gamepad1.left_stick_y > 0) {
            motor.setPower(1.0);
        } else {
            motor.setPower(0.0);
        }
    }
}</code></pre>

      <h2>Uploading and Running Your Code</h2>
      <p><span class="badge wip">WIP</span> — This section is coming soon.</p>

      <div class="challenges">
        <h3>Challenges</h3>
        <ul>
          <li>Make the motor spin backwards</li>
          <li>Make the motor speed based on how far forward the joystick is pushed. <em>(Hint: You can put variables into <code>.setPower()</code>)</em></li>
          <li>Add a second motor!</li>
        </ul>
      </div>

    </article>

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
