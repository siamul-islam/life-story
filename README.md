<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>EEE · Section B — Routine &amp; Tasks</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500;600;700&family=IBM+Plex+Sans:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#0a2e45;
    --bg-2:#082638;
    --surface:#123b57;
    --surface-2:#0e3450;
    --paper:#eef0e6;
    --paper-line:#d8d9cb;
    --ink:#eaf2f7;
    --ink-dim:#cfe1ec;
    --muted:#82a8c2;
    --accent:#e8a33d;
    --accent-dim:#9c712b;
    --good:#6fbf8b;
    --bad:#d9706a;
    --line:rgba(255,255,255,0.14);
    --grid:rgba(255,255,255,0.045);
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:
      linear-gradient(var(--grid) 1px, transparent 1px) 0 0/28px 28px,
      linear-gradient(90deg, var(--grid) 1px, transparent 1px) 0 0/28px 28px,
      radial-gradient(ellipse at top, var(--bg-2), var(--bg) 60%);
    color:var(--ink);
    font-family:'IBM Plex Sans', sans-serif;
    min-height:100vh;
    padding-bottom:60px;
  }
  .wrap{max-width:780px;margin:0 auto;padding:22px 16px 0;}

  /* ---- Header ---- */
  header{
    display:flex; justify-content:space-between; align-items:flex-start;
    gap:12px; margin-bottom:22px;
  }
  .title-block .eyebrow{
    font-family:'IBM Plex Mono', monospace;
    font-size:11px; letter-spacing:.18em; color:var(--accent);
    text-transform:uppercase; margin:0 0 4px;
  }
  .title-block h1{
    margin:0; font-family:'IBM Plex Mono', monospace;
    font-size:24px; font-weight:700; color:var(--ink); letter-spacing:.01em;
  }
  .title-block p{margin:4px 0 0; color:var(--muted); font-size:13px;}
  .clock{
    text-align:right; font-family:'IBM Plex Mono', monospace;
    border:1px solid var(--line); border-radius:8px; padding:10px 14px;
    background:rgba(0,0,0,0.15); min-width:128px;
  }
  .clock .t{font-size:20px; font-weight:600; color:var(--ink);}
  .clock .d{font-size:11px; color:var(--muted); margin-top:2px;}
  .led-row{display:flex; align-items:center; gap:6px; justify-content:flex-end; margin-top:6px;}
  .led{width:8px;height:8px;border-radius:50%;background:var(--muted);}
  .led.on{background:var(--accent); box-shadow:0 0 8px 2px var(--accent); animation:pulse 1.6s infinite;}
  .led-row span{font-size:10px; letter-spacing:.1em; color:var(--muted); text-transform:uppercase;}
  @keyframes pulse{0%,100%{opacity:1;}50%{opacity:.35;}}

  /* ---- Day tabs ---- */
  .tabs{display:flex; gap:6px; margin-bottom:18px; overflow-x:auto; padding-bottom:2px;}
  .tab{
    font-family:'IBM Plex Mono', monospace; font-size:12px; letter-spacing:.04em;
    padding:8px 14px; border-radius:7px; border:1px solid var(--line);
    background:var(--surface-2); color:var(--ink-dim); cursor:pointer; white-space:nowrap;
    transition:all .15s ease;
  }
  .tab:hover{border-color:var(--accent-dim);}
  .tab.active{background:var(--accent); color:#1a1206; border-color:var(--accent); font-weight:600;}
  .tab.today::after{content:" •"; color:inherit;}

  /* ---- Schedule panel ---- */
  .panel{
    background:var(--surface); border:1px solid var(--line); border-radius:12px;
    padding:18px 18px 8px; margin-bottom:26px; position:relative;
  }
  .panel h2{
    font-family:'IBM Plex Mono', monospace; font-size:13px; letter-spacing:.12em;
    text-transform:uppercase; color:var(--muted); margin:0 0 14px;
  }
  .timeline{position:relative; padding-left:22px;}
  .timeline::before{
    content:""; position:absolute; left:5px; top:6px; bottom:6px; width:2px;
    background:linear-gradient(to bottom, var(--accent-dim), var(--line));
  }
  .node{position:relative; padding-bottom:16px;}
  .node:last-child{padding-bottom:0;}
  .node .pin{
    position:absolute; left:-22px; top:4px; width:12px; height:12px; border-radius:50%;
    background:var(--surface-2); border:2px solid var(--muted);
  }
  .node.now .pin{
    background:var(--accent); border-color:var(--accent);
    box-shadow:0 0 0 4px rgba(232,163,61,0.25); animation:pulse 1.6s infinite;
  }
  .node.done .pin{background:var(--good); border-color:var(--good);}
  .card{
    border:1px solid var(--line); border-radius:9px; padding:11px 13px;
    background:var(--surface-2); transition:border-color .15s ease;
  }
  .node.now .card{border-color:var(--accent); box-shadow:0 0 0 1px rgba(232,163,61,0.15) inset;}
  .card-top{display:flex; justify-content:space-between; align-items:baseline; gap:10px;}
  .course{font-family:'IBM Plex Mono', monospace; font-weight:600; font-size:14.5px; color:var(--ink);}
  .badge{
    font-family:'IBM Plex Mono', monospace; font-size:10px; letter-spacing:.06em;
    padding:2px 7px; border-radius:5px; text-transform:uppercase;
  }
  .badge.lab{background:rgba(232,163,61,0.18); color:var(--accent);}
  .badge.theory{background:rgba(130,168,194,0.18); color:var(--ink-dim);}
  .time-row{
    font-family:'IBM Plex Mono', monospace; font-size:11.5px; color:var(--muted); margin-top:3px;
    display:flex; gap:10px;
  }
  .empty-day{
    color:var(--muted); font-size:13px; padding:10px 0 6px; font-style:italic;
  }

  /* ---- Legend ---- */
  details.legend{margin-bottom:26px;}
  details.legend summary{
    cursor:pointer; font-family:'IBM Plex Mono', monospace; font-size:12px;
    letter-spacing:.08em; text-transform:uppercase; color:var(--muted); padding:6px 2px;
  }
  .legend-grid{
    display:grid; grid-template-columns:1fr 1fr; gap:6px 16px; margin-top:10px;
    font-size:12.5px; color:var(--ink-dim);
  }
  .legend-grid b{font-family:'IBM Plex Mono', monospace; color:var(--accent); margin-right:4px;}

  /* ---- Task / component queue ---- */
  .panel.tasks{padding-bottom:18px;}
  .task-list{display:flex; flex-direction:column; gap:8px; margin-bottom:14px;}
  .task{
    display:flex; align-items:flex-start; gap:10px;
    background:var(--paper); color:#23271f; border-radius:8px; padding:10px 12px;
    border-left:4px solid var(--accent-dim);
  }
  .task.done{opacity:.55; border-left-color:var(--good);}
  .task.done .task-title{text-decoration:line-through;}
  .check{
    width:18px;height:18px;border-radius:4px;border:2px solid #5b5f50;
    flex:none; margin-top:1px; cursor:pointer; display:flex; align-items:center; justify-content:center;
    background:#fff;
  }
  .task.done .check{background:var(--good); border-color:var(--good); color:#fff;}
  .task-body{flex:1; min-width:0;}
  .task-title{font-size:13.5px; font-weight:500; word-break:break-word;}
  .task-meta{font-family:'IBM Plex Mono', monospace; font-size:10.5px; color:#5b6b58; margin-top:2px; text-transform:uppercase; letter-spacing:.04em;}
  .task-del{
    border:none; background:none; color:#8a4a44; cursor:pointer; font-size:16px; line-height:1;
    padding:2px 4px; flex:none;
  }
  .add-form{display:flex; gap:8px; flex-wrap:wrap;}
  .add-form input, .add-form select{
    font-family:'IBM Plex Sans', sans-serif; font-size:13px; padding:9px 11px;
    border-radius:7px; border:1px solid var(--line); background:var(--surface-2); color:var(--ink);
  }
  .add-form input::placeholder{color:var(--muted);}
  .add-form input[name="title"]{flex:1; min-width:140px;}
  .add-form button{
    font-family:'IBM Plex Mono', monospace; font-weight:600; font-size:12.5px;
    background:var(--accent); color:#1a1206; border:none; border-radius:7px;
    padding:9px 16px; cursor:pointer; letter-spacing:.04em;
  }
  .add-form button:hover{background:#f0ae4d;}
  .empty-tasks{color:var(--muted); font-size:13px; padding:6px 0 12px;}
  .status-line{font-size:11px; color:var(--muted); text-align:right; margin-top:-4px;}

  @media (max-width:480px){
    .title-block h1{font-size:19px;}
    .clock{min-width:108px; padding:8px 10px;}
    .add-form{flex-direction:column;}
  }
  @media (prefers-reduced-motion: reduce){
    .led.on, .node.now .pin{animation:none;}
  }
</style>
</head>
<body>
<div class="wrap">

  <header>
    <div class="title-block">
      <p class="eyebrow">KUET · Dept. of EEE</p>
      <h1>Section B — Room 105</h1>
      <p>1st Year, 1st Term · class routine + task board</p>
    </div>
    <div class="clock">
      <div class="t" id="clockTime">--:--</div>
      <div class="d" id="clockDate">—</div>
      <div class="led-row"><span id="ledLabel">checking</span><div class="led" id="led"></div></div>
    </div>
  </header>

  <div class="tabs" id="tabs"></div>

  <div class="panel">
    <h2 id="dayHeading">Schedule</h2>
    <div class="timeline" id="timeline"></div>
  </div>

  <details class="legend">
    <summary>Faculty key (initials)</summary>
    <div class="legend-grid" id="legendGrid"></div>
  </details>

  <div class="panel tasks">
    <h2>Component Queue — Tasks</h2>
    <div class="task-list" id="taskList"></div>
    <div class="empty-tasks" id="emptyTasks" style="display:none;">No tasks queued. Add one below.</div>
    <form class="add-form" id="addForm">
      <input type="text" name="title" placeholder="Task — e.g. Math 1103 problem set" required />
      <select name="day">
        <option value="">No day</option>
        <option>Sunday</option><option>Monday</option><option>Tuesday</option>
        <option>Wednesday</option><option>Thursday</option><option>Friday</option><option>Saturday</option>
      </select>
      <button type="submit">Add</button>
    </form>
    <div class="status-line" id="storageStatus"></div>
  </div>

</div>

<script>
/* ---------------- Schedule data — Section B ---------------- */
const schedule = {
  Sunday: [
    {period:"1", time:"08:00–08:50", start:"08:00", end:"08:50", course:"EE 1103", teacher:"RI2 / SUY", type:"theory"},
    {period:"2", time:"08:50–09:40", start:"08:50", end:"09:40", course:"Math 1103", teacher:"RI / HR", type:"theory"},
    {period:"3", time:"09:40–10:30", start:"09:40", end:"10:30", course:"Phy 1103", teacher:"JS / AZ", type:"theory"},
  ],
  Monday: [
    {period:"1–3", time:"08:00–10:30", start:"08:00", end:"10:30", course:"EE 1104", teacher:"RI2 + MJI", room:"B1", type:"lab"},
    {period:"4", time:"10:40–11:30", start:"10:40", end:"11:30", course:"Math 1103", teacher:"RI / HR", type:"theory"},
    {period:"5", time:"11:30–12:20", start:"11:30", end:"12:20", course:"Chem 1103", teacher:"AY / HM", type:"theory"},
    {period:"6", time:"12:20–13:10", start:"12:20", end:"13:10", course:"Hum 1103", teacher:"MB / TBA", type:"theory"},
    {period:"7–8", time:"14:30–16:10", start:"14:30", end:"16:10", course:"CE 1104", teacher:"MM + MSH", room:"B2/B1", type:"lab"},
  ],
  Tuesday: [
    {period:"1", time:"08:00–08:50", start:"08:00", end:"08:50", course:"Phy 1103", teacher:"JS / AZ", type:"theory"},
    {period:"2", time:"08:50–09:40", start:"08:50", end:"09:40", course:"EE 1103", teacher:"RI2 / SUY", type:"theory"},
    {period:"3", time:"09:40–10:30", start:"09:40", end:"10:30", course:"Chem 1103", teacher:"AY / HM", type:"theory"},
    {period:"4–6", time:"10:40–13:10", start:"10:40", end:"13:10", course:"Phy 1104", teacher:"MA + SDN", room:"B1/B2", type:"lab"},
    {period:"7–9", time:"14:30–17:00", start:"14:30", end:"17:00", course:"EE 1104", teacher:"RI2 + MJI", room:"B2", type:"lab"},
  ],
  Wednesday: [
    {period:"1", time:"08:00–08:50", start:"08:00", end:"08:50", course:"Chem 1103", teacher:"AY / HM", type:"theory"},
    {period:"2", time:"08:50–09:40", start:"08:50", end:"09:40", course:"Hum 1103", teacher:"MB / TBA", type:"theory"},
    {period:"3", time:"09:40–10:30", start:"09:40", end:"10:30", course:"EE 1103", teacher:"RI2 / SUY", type:"theory"},
    {period:"4–6", time:"10:40–13:10", start:"10:40", end:"13:10", course:"CHEM 1104", teacher:"AM + PR", room:"B2/B1", type:"lab"},
  ],
  Thursday: [
    {period:"2", time:"08:50–09:40", start:"08:50", end:"09:40", course:"Chem 1103", teacher:"AY / HM", room:"312", type:"theory"},
    {period:"4", time:"10:40–11:30", start:"10:40", end:"11:30", course:"Hum 1103", teacher:"MB / TBA", type:"theory"},
    {period:"5", time:"11:30–12:20", start:"11:30", end:"12:20", course:"Phy 1103", teacher:"JS / AZ", type:"theory"},
    {period:"6", time:"12:20–13:10", start:"12:20", end:"13:10", course:"Math 1103", teacher:"HR / RI", type:"theory"},
  ],
  Friday: [],
  Saturday: [],
};

const legend = {
  "AGB":"Dr. Ashraful Ghani Bhuiyan","RI2":"Dr. Md. Rafiqul Islam 2","SUY":"Dr. Salah Uddin Yousuf",
  "MJI":"Dr. Md. Jahirul Islam","MA":"Dr. Md. Mahbub Alam","JS":"Dr. Jolly Sultana",
  "AZ":"Dr. Md. Asaduzzaman","SDN":"Sumon Deb Nath","AY":"Dr. Mohammad Abu Yousuf",
  "AM":"Dr. Md. Abdul Motin","HM":"Dr. Mohammad Hasan Morshed","PR":"Dr. Parbhej Ahamed",
  "HR":"Dr. Md. Habibur Rahman","RI":"Dr. Rafiqul Islam","MB":"Maria Bhuiyan",
  "MM":"Md. Mamun Mahmud","MSH":"Md. Shabbir Hossain","TBA":"To Be Appointed"
};

const days = ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"];
let activeDay = days[new Date().getDay()];

/* ---------------- Render tabs ---------------- */
const tabsEl = document.getElementById('tabs');
function renderTabs(){
  tabsEl.innerHTML = "";
  const todayName = days[new Date().getDay()];
  days.forEach(d=>{
    const b = document.createElement('button');
    b.className = 'tab' + (d===activeDay ? ' active':'') + (d===todayName ? ' today':'');
    b.textContent = d.slice(0,3);
    b.onclick = ()=>{ activeDay = d; renderTabs(); renderSchedule(); };
    tabsEl.appendChild(b);
  });
}

/* ---------------- Time helpers ---------------- */
function toMinutes(hhmm){ const [h,m]=hhmm.split(':').map(Number); return h*60+m; }
function nowMinutes(){ const n=new Date(); return n.getHours()*60+n.getMinutes(); }

/* ---------------- Render schedule ---------------- */
function renderSchedule(){
  document.getElementById('dayHeading').textContent = activeDay + " — schedule";
  const timeline = document.getElementById('timeline');
  const items = schedule[activeDay] || [];
  const todayName = days[new Date().getDay()];
  const nm = nowMinutes();

  if(items.length===0){
    timeline.innerHTML = '<div class="empty-day">No scheduled classes — free day.</div>';
    return;
  }

  timeline.innerHTML = items.map(it=>{
    let state = '';
    if(activeDay===todayName){
      if(nm >= toMinutes(it.start) && nm < toMinutes(it.end)) state='now';
      else if(nm >= toMinutes(it.end)) state='done';
    }
    const roomTxt = it.room ? ` · Room ${it.room}` : '';
    return `
      <div class="node ${state}">
        <div class="pin"></div>
        <div class="card">
          <div class="card-top">
            <span class="course">${it.course}</span>
            <span class="badge ${it.type}">${it.type}</span>
          </div>
          <div class="time-row">
            <span>P${it.period}</span><span>${it.time}</span><span>${it.teacher}${roomTxt}</span>
          </div>
        </div>
      </div>`;
  }).join('');
}

/* ---------------- Clock + live indicator ---------------- */
function renderClock(){
  const n = new Date();
  document.getElementById('clockTime').textContent = n.toLocaleTimeString([], {hour:'2-digit', minute:'2-digit'});
  document.getElementById('clockDate').textContent = n.toLocaleDateString([], {weekday:'short', month:'short', day:'numeric'});

  const todayName = days[n.getDay()];
  const items = schedule[todayName] || [];
  const nm = nowMinutes();
  const inClass = items.some(it => nm >= toMinutes(it.start) && nm < toMinutes(it.end));
  document.getElementById('led').className = 'led' + (inClass ? ' on' : '');
  document.getElementById('ledLabel').textContent = inClass ? 'in class' : 'free now';

  // re-render schedule highlight if viewing today
  if(activeDay===todayName) renderSchedule();
}

/* ---------------- Legend ---------------- */
document.getElementById('legendGrid').innerHTML = Object.entries(legend)
  .map(([k,v])=>`<div><b>${k}</b>${v}</div>`).join('');

/* ---------------- Tasks (persisted via window.storage) ---------------- */
const TASKS_KEY = 'routine:tasks';
let tasks = [];
let storageOK = true;

async function loadTasks(){
  try{
    const res = await window.storage.get(TASKS_KEY);
    tasks = res && res.value ? JSON.parse(res.value) : [];
  }catch(e){
    tasks = [];
  }
  renderTasks();
}
async function saveTasks(){
  try{
    await window.storage.set(TASKS_KEY, JSON.stringify(tasks));
    document.getElementById('storageStatus').textContent = '';
  }catch(e){
    storageOK = false;
    document.getElementById('storageStatus').textContent = 'Storage unavailable — tasks won\'t persist this session.';
  }
}

function renderTasks(){
  const list = document.getElementById('taskList');
  const empty = document.getElementById('emptyTasks');
  if(tasks.length===0){
    list.innerHTML=''; empty.style.display='block'; return;
  }
  empty.style.display='none';
  list.innerHTML = tasks.map((t,i)=>`
    <div class="task ${t.done?'done':''}">
      <div class="check" data-i="${i}">${t.done?'✓':''}</div>
      <div class="task-body">
        <div class="task-title">${escapeHtml(t.title)}</div>
        <div class="task-meta">${t.day ? t.day : 'no day set'}</div>
      </div>
      <button class="task-del" data-del="${i}" aria-label="Delete task">×</button>
    </div>
  `).join('');

  list.querySelectorAll('.check').forEach(el=>{
    el.onclick = ()=>{
      const i = +el.dataset.i;
      tasks[i].done = !tasks[i].done;
      renderTasks(); saveTasks();
    };
  });
  list.querySelectorAll('[data-del]').forEach(el=>{
    el.onclick = ()=>{
      const i = +el.dataset.del;
      tasks.splice(i,1);
      renderTasks(); saveTasks();
    };
  });
}

function escapeHtml(s){
  const d = document.createElement('div'); d.textContent = s; return d.innerHTML;
}

document.getElementById('addForm').addEventListener('submit', (e)=>{
  e.preventDefault();
  const form = e.target;
  const title = form.title.value.trim();
  const day = form.day.value;
  if(!title) return;
  tasks.unshift({title, day, done:false});
  form.title.value=''; form.day.value='';
  renderTasks(); saveTasks();
});

/* ---------------- Init ---------------- */
renderTabs();
renderSchedule();
renderClock();
setInterval(renderClock, 30000);
loadTasks();
</script>
</body>
</html>

