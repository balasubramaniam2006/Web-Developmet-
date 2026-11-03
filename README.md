<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Earth Checkup</title>
<style>
:root{
  --bg:#071019; --panel:#0e1b2b; --panel2:#122235; --text:#eaf6ff; --sub:#8fa8bd;
  --accent:#36e2b4; --accent2:#4fa8ff; --warn:#ffc156; --bad:#ff6b6b; --good:#36e2b4;
  --border:rgba(255,255,255,0.08); color-scheme: dark;
}
@media (prefers-color-scheme: light){
  :root:not([data-theme="dark"]){--bg:#f4f8fb;--panel:#ffffff;--panel2:#eef3f7;--text:#102033;--sub:#5a7187;--border:rgba(0,0,0,0.08);}
}
*{box-sizing:border-box;}
html,body{height:100%;margin:0;}
body::before{content:"";position:fixed;inset:0 0 auto;height:env(safe-area-inset-top,0px);z-index:10;background:var(--bg);}
body{
  font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
  background:radial-gradient(1200px 600px at 20% -10%, #0e2a3f 0%, var(--bg) 60%),
             radial-gradient(900px 500px at 100% 0%, #0e3a2f 0%, var(--bg) 55%), var(--bg);
  color:var(--text);
  padding-top:env(safe-area-inset-top,0px); padding-bottom:env(safe-area-inset-bottom,0px);
  min-height:100%;
}
#app{max-width:980px;margin:0 auto;padding:32px 20px 60px;}

/* ---------- LOGIN ---------- */
#login-screen{min-height:100vh;display:flex;align-items:center;justify-content:center;padding:20px;}
.login-card{
  width:100%;max-width:380px;background:linear-gradient(180deg,var(--panel),var(--panel2));
  border:1px solid var(--border);border-radius:20px;padding:36px 28px;
  box-shadow:0 20px 60px rgba(0,0,0,0.4);
}
.earth-logo{
  width:64px;height:64px;border-radius:50%;margin:0 auto 18px;
  background:conic-gradient(from 210deg, #1b5e3f, #2d8f5f, #1d6fae, #2d8f5f, #1b5e3f);
  box-shadow:0 0 40px rgba(54,226,180,0.35), inset -6px -6px 14px rgba(0,0,0,0.4);
  animation:spin 14s linear infinite;
}
@keyframes spin{to{transform:rotate(360deg);}}
.login-card h1{font-size:22px;text-align:center;margin:0 0 4px;letter-spacing:0.3px;}
.login-card p.tag{text-align:center;color:var(--sub);font-size:13px;margin:0 0 24px;}
.field{margin-bottom:14px;}
.field label{display:block;font-size:12px;color:var(--sub);margin-bottom:6px;}
.field input{
  width:100%;padding:11px 13px;border-radius:10px;border:1px solid var(--border);
  background:rgba(255,255,255,0.03);color:var(--text);font-size:14px;outline:none;
}
.field input:focus{border-color:var(--accent2);}
.btn{
  width:100%;padding:12px;border-radius:10px;border:none;font-size:14px;font-weight:600;
  cursor:pointer;margin-top:6px;transition:transform .15s ease;
}
.btn:active{transform:scale(0.98);}
.btn-primary{background:linear-gradient(90deg,var(--accent2),var(--accent));color:#06131a;}
.btn-link{background:transparent;color:var(--accent2);font-weight:500;margin-top:14px;}
.error-msg{color:var(--bad);font-size:12px;text-align:center;min-height:16px;margin-top:4px;}
.divider{text-align:center;color:var(--sub);font-size:11px;margin:18px 0;}

/* ---------- DASHBOARD ---------- */
#dashboard{display:none;}
header.top{display:flex;align-items:center;justify-content:space-between;margin-bottom:26px;}
header.top .who{font-size:13px;color:var(--sub);}
header.top h1{font-size:24px;margin:2px 0 0;}
.signout{background:transparent;border:1px solid var(--border);color:var(--sub);padding:8px 14px;border-radius:9px;font-size:12px;cursor:pointer;}

.score-band{
  display:flex;align-items:center;gap:22px;background:linear-gradient(135deg,var(--panel),var(--panel2));
  border:1px solid var(--border);border-radius:18px;padding:22px;margin-bottom:22px;
}
.score-ring{flex:0 0 auto;}
.score-text h2{margin:0;font-size:15px;color:var(--sub);font-weight:500;}
.score-text .big{font-size:30px;font-weight:700;margin:4px 0 2px;}
.score-text .desc{font-size:13px;color:var(--sub);}

.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:14px;}
.card{
  background:linear-gradient(180deg,var(--panel),var(--panel2));border:1px solid var(--border);
  border-radius:16px;padding:18px;position:relative;overflow:hidden;
}
.card .icon{font-size:20px;}
.card .label{font-size:12px;color:var(--sub);margin-top:8px;}
.card .val{font-size:22px;font-weight:700;margin-top:2px;}
.card .delta{font-size:11px;margin-top:4px;display:inline-block;padding:2px 7px;border-radius:20px;}
.delta.up{color:#ff8b8b;background:rgba(255,107,107,0.12);}
.delta.down{color:#5fe3c0;background:rgba(54,226,180,0.12);}
.bar-track{height:6px;border-radius:4px;background:rgba(255,255,255,0.08);margin-top:12px;overflow:hidden;}
.bar-fill{height:100%;border-radius:4px;}

footer.note{color:var(--sub);font-size:11px;text-align:center;margin-top:30px;line-height:1.6;}
</style>
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

<script>
const USERS_KEY = 'ec_users';
const SESSION_KEY = 'ec_session';
let mode = 'signin';

function getUsers(){ try{ return JSON.parse(localStorage.getItem(USERS_KEY)) || {}; }catch(e){ return {}; } }
function saveUsers(u){ try{ localStorage.setItem(USERS_KEY, JSON.stringify(u)); }catch(e){} }

function toggleMode(){
  mode = mode === 'signin' ? 'signup' : 'signin';
  document.getElementById('form-title').textContent = mode === 'signin' ? 'Welcome back' : 'Create your account';
  document.getElementById('in-name').style.display = mode === 'signup' ? 'block' : 'none';
  document.getElementById('submit-btn').textContent = mode === 'signin' ? 'Sign In' : 'Sign Up';
  document.getElementById('toggle-btn').textContent = mode === 'signin' ? 'Need an account? Sign up' : 'Have an account? Sign in';
  document.getElementById('err').textContent = '';
}

function submitAuth(){
  const email = document.getElementById('in-email').value.trim().toLowerCase();
  const pass = document.getElementById('in-pass').value;
  const name = document.getElementById('in-name').value.trim();
  const err = document.getElementById('err');
  err.textContent = '';
  if(!email || !pass){ err.textContent = 'Please fill in email and password.'; return; }

  const users = getUsers();
  if(mode === 'signup'){
    if(users[email]){ err.textContent = 'An account with that email already exists.'; return; }
    users[email] = { name: name || email.split('@')[0], pass };
    saveUsers(users);
    startSession(email, users[email].name);
  } else {
    const u = users[email];
    if(!u || u.pass !== pass){ err.textContent = 'Incorrect email or password.'; return; }
    startSession(email, u.name);
  }
}

function startSession(email, name){
  try{ localStorage.setItem(SESSION_KEY, JSON.stringify({email,name})); }catch(e){}
  showDashboard(name);
}
function signOut(){
  try{ localStorage.removeItem(SESSION_KEY); }catch(e){}
  document.getElementById('dashboard').style.display = 'none';
  document.getElementById('login-screen').style.display = 'flex';
}

function showDashboard(name){
  document.getElementById('login-screen').style.display = 'none';
  document.getElementById('dashboard').style.display = 'block';
  document.getElementById('who-name').textContent = name;
  renderVitals();
}

const VITALS = [
  {icon:'🌡️', label:'Global Temp Anomaly', val:'+1.34°C', pct:67, up:true, color:'#ff8b6b'},
  {icon:'🫁', label:'Atmospheric CO₂', val:'424 ppm', pct:82, up:true, color:'#ff6b6b'},
  {icon:'🧊', label:'Arctic Ice Extent', val:'-12.6% vs avg', pct:38, up:false, color:'#4fa8ff'},
  {icon:'🌊', label:'Sea Level Rise', val:'+3.4 mm/yr', pct:55, up:true, color:'#4fc8ff'},
  {icon:'🫧', label:'Ocean Acidity (pH)', val:'8.06', pct:60, up:false, color:'#36e2b4'},
  {icon:'🌳', label:'Forest Cover Change', val:'-0.3%/yr', pct:44, up:false, color:'#6bd98f'},
  {icon:'💨', label:'Global Air Quality', val:'Moderate (AQI 58)', pct:58, up:null, color:'#ffc156'},
  {icon:'🔥', label:'Wildfire Activity', val:'Above average', pct:71, up:true, color:'#ff9f4f'},
];

function renderVitals(){
  const grid = document.getElementById('vitals-grid');
  grid.innerHTML = '';
  VITALS.forEach(v=>{
    const deltaClass = v.up === null ? '' : (v.up ? 'up' : 'down');
    const deltaIcon = v.up === null ? '' : (v.up ? '▲' : '▼');
    const card = document.createElement('div');
    card.className = 'card';
    card.innerHTML = `
      <div class="icon">${v.icon}</div>
      <div class="label">${v.label}</div>
      <div class="val">${v.val}</div>
      ${v.up!==null ? `<span class="delta ${deltaClass}">${deltaIcon} trend</span>` : ''}
      <div class="bar-track"><div class="bar-fill" style="width:${v.pct}%;background:${v.color}"></div></div>
    `;
    grid.appendChild(card);
  });

  const avg = Math.round(VITALS.reduce((s,v)=>s+v.pct,0)/VITALS.length);
  const score = 100 - avg;
  const circumference = 2*Math.PI*46;
  const arc = document.getElementById('score-arc');
  arc.style.strokeDasharray = circumference;
  arc.style.strokeDashoffset = circumference - (score/100)*circumference;
  arc.style.transition = 'stroke-dashoffset 1s ease';
  document.getElementById('score-num').textContent = score;
}

(function init(){
  try{
    const s = JSON.parse(localStorage.getItem(SESSION_KEY));
    if(s && s.email) showDashboard(s.name);
  }catch(e){}
})();
</script>
</body>
</html>
