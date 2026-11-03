<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Earth Checkup</title>
<link rel="stylesheet" href="style.css">
</head>
<body>
<div id="app">

  <div id="login-screen">
    <div class="login-card">
      <div class="earth-logo"></div>
      <h1 id="form-title">Welcome back</h1>
      <p class="tag">Sign in to run your Earth Checkup</p>
      <div class="field"><label>Name</label><input id="in-name" type="text" placeholder="Ada Lovelace" style="display:none"></div>
      <div class="field"><label>Email</label><input id="in-email" type="email" placeholder="you@example.com"></div>
      <div class="field"><label>Password</label><input id="in-pass" type="password" placeholder="••••••••"></div>
      <div class="error-msg" id="err"></div>
      <button class="btn btn-primary" id="submit-btn" onclick="submitAuth()">Sign In</button>
      <button class="btn btn-link" onclick="toggleMode()" id="toggle-btn">Need an account? Sign up</button>
      <div class="divider">— local demo auth, no server involved —</div>
    </div>
  </div>

  <div id="dashboard">
    <header class="top">
      <div><div class="who">Signed in as <span id="who-name"></span></div><h1>🌍 Earth Checkup</h1></div>
      <button class="signout" onclick="signOut()">Sign out</button>
    </header>

    <div class="score-band">
      <svg class="score-ring" width="110" height="110" viewBox="0 0 110 110">
        <circle cx="55" cy="55" r="46" fill="none" stroke="rgba(255,255,255,0.08)" stroke-width="10"/>
        <circle id="score-arc" cx="55" cy="55" r="46" fill="none" stroke="url(#g1)" stroke-width="10" stroke-linecap="round" transform="rotate(-90 55 55)"/>
        <defs><linearGradient id="g1" x1="0" y1="0" x2="1" y2="1">
          <stop offset="0%" stop-color="#4fa8ff"/><stop offset="100%" stop-color="#36e2b4"/>
        </linearGradient></defs>
        <text x="55" y="61" text-anchor="middle" font-size="22" font-weight="700" fill="var(--text)" id="score-num">62</text>
      </svg>
      <div class="score-text">
        <h2>Planet Vitals Score</h2>
        <div class="big">Fair condition</div>
        <div class="desc">Based on live-style readings across temperature, air, ice, and ocean systems. Refresh to re-sample.</div>
      </div>
    </div>

    <div class="grid" id="vitals-grid"></div>

    <footer class="note">
      Figures are illustrative demo values for UI purposes, not live sensor data.<br>
      Earth Checkup · client-side only · no account data leaves this browser
    </footer>
  </div>
</div>

<script src="script.js"></script>
</body>
</html>
