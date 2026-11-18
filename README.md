<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SECURITY QUIZ</title>
<style>
*{margin:0;padding:0;box-sizing:border-box}
:root{--bg:#0a0d14;--panel:#121827;--line:#243049;--txt:#eaf0ff;--mut:#788599;--acc:#00bfff}
body{min-height:100vh;background:radial-gradient(circle at 50% 0,#12203a,#0a0d14 60%);color:var(--txt);font-family:Arial,sans-serif;display:flex;justify-content:center;padding:20px}
.app{width:100%;max-width:760px}
h1{text-align:center;letter-spacing:4px;font-size:30px;text-shadow:0 0 15px rgba(0,174,255,.45);margin:6px 0 4px}
.sub{text-align:center;color:var(--mut);font-size:12px;letter-spacing:2px;margin-bottom:18px}
.card{background:var(--panel);border:1px solid var(--line);border-radius:14px;padding:20px}
.hidden{display:none!important}
label{display:block;font-size:12px;color:var(--mut);letter-spacing:1px;margin:14px 0 6px}
input[type=text]{width:100%;padding:10px;border-radius:8px;border:1px solid var(--line);background:#0c1220;color:var(--txt);font-size:15px;margin-bottom:6px}
.row{display:flex;gap:8px;flex-wrap:wrap}
button{cursor:pointer;font-family:inherit;color:var(--txt);border:1px solid var(--line);background:#182238;border-radius:10px;padding:10px 16px;font-size:15px;transition:.15s}
button:hover{border-color:var(--acc);box-shadow:0 0 12px rgba(0,191,255,.25)}
button.sel{background:var(--acc);color:#001018;border-color:var(--acc);font-weight:700}
.primary{background:linear-gradient(90deg,#007bff,#00bfff);border:none;color:#001018;font-weight:700;width:100%;margin-top:18px;padding:13px;letter-spacing:2px}
.legend{display:flex;flex-wrap:wrap;gap:8px;justify-content:center;margin:12px 0}
.tag{display:flex;align-items:center;gap:6px;font-size:12px;padding:4px 10px;border-radius:20px;border:1px solid var(--line);background:#0c1220}
.dot{width:12px;height:12px;border-radius:50%;border:1px solid #fff5}
.scores{display:flex;gap:8px;flex-wrap:wrap;justify-content:center;margin-bottom:12px}
.pl{padding:8px 14px;border-radius:10px;border:2px solid var(--line);background:#0c1220;font-size:14px}
.pl.on{box-shadow:0 0 14px currentColor}
.board{display:grid;grid-template-columns:repeat(5,1fr);gap:8px}
.cell{aspect-ratio:1;border-radius:12px;border:2px solid #ffffff30;display:flex;flex-direction:column;align-items:center;justify-content:center;font-size:clamp(20px,5vw,34px);cursor:pointer;position:relative;transition:.15s;padding:0;color:#fff}
.cell:hover{transform:scale(1.06);box-shadow:0 0 16px #fff4}
.cell small{font-size:10px;letter-spacing:1px;opacity:.85;margin-top:2px}
.cell.done{cursor:default;opacity:.55;transform:none;box-shadow:none}
.cell .own{position:absolute;top:4px;right:4px;width:22px;height:22px;border-radius:50%;font-size:12px;font-weight:700;display:flex;align-items:center;justify-content:center;color:#111;border:2px solid #fff}
.modal{position:fixed;inset:0;background:#000a;display:flex;align-items:center;justify-content:center;padding:16px;z-index:5}
.box{background:var(--panel);border-radius:16px;border:2px solid var(--line);padding:22px;width:100%;max-width:520px;animation:pop .25s ease-out}
@keyframes pop{from{opacity:0;transform:scale(.94)}to{opacity:1;transform:scale(1)}}
.theme{display:inline-block;padding:5px 14px;border-radius:20px;font-weight:700;font-size:13px;letter-spacing:1px;margin-bottom:12px;border:1px solid #fff6}
.q{font-size:18px;line-height:1.4;margin:6px 0 14px}
.opts{display:grid;gap:8px}
.opts button{text-align:left}
.opts button.ok{background:#1b7a3a;border-color:#4cff88}
.opts button.ko{background:#8a1f1f;border-color:#ff6b6b}
.bar{height:6px;background:#0c1220;border-radius:4px;overflow:hidden;margin-bottom:12px}
.bar i{display:block;height:100%;background:var(--acc);width:100%}
.diff button{flex:1;min-width:90px}
.fb{margin-top:12px;font-weight:700}
.note{font-size:11px;color:var(--mut);margin-top:10px;text-align:center}
</style>
</head>
<body>
<div class="app">
  <h1>SECURITY QUIZ</h1>
  <div class="sub">CONQUIERS LES 25 CASES</div>

  <!-- ACCUEIL -->
  <div id="setup" class="card">
    <label>NOMBRE DE JOUEURS</label>
    <div class="row" id="nbBtns"></div>
    <div id="names"></div>
    <div class="legend" id="legend"></div>
    <p class="note">Choisis une case, puis un niveau (1 facile → 3 très difficile). Tape ta réponse : si elle ressemble à au moins 80 % à la bonne, la case est à toi (1, 2 ou 3 points).</p>
    <button class="primary" id="start">JOUER</button>
  </div>

  <!-- JEU -->
  <div id="game" class="hidden">
    <div class="scores" id="scores"></div>
    <div class="board" id="board"></div>
    <div class="legend" id="legend2"></div>
  </div>

  <!-- FIN -->
  <div id="end" class="card hidden" style="text-align:center">
    <h2 id="winner" style="margin-bottom:12px"></h2>
    <div id="finalScores"></div>
    <button class="primary" onclick="location.reload()">REJOUER</button>
  </div>
</div>

<div id="modal" class="modal hidden"><div class="box" id="box"></div></div>

<script src="questions.js"></script>
<script>
/* ============ THÈMES ============ */
const THEMES = [
  {id:'pompier', name:'Pompiers',          color:'#e53935', icon:'🚒'},
  {id:'police',  name:'Police',            color:'#1a3a8f', icon:'👮'},
  {id:'armee',   name:'Armée',             color:'#2e7d32', icon:'🪖'},
  {id:'gendarm', name:'Gendarmerie',       color:'#4fc3f7', icon:'🛡️'},
  {id:'prive',   name:'Sécurité privée',   color:'#111111', icon:'🕶️'},
];
const PCOLORS = ['#ffd54f','#f48fb1','#ffab40','#b39ddb'];
const TIMES = {1:45, 2:40, 3:30};

/* QUESTIONS est défini dans questions.js */

const used = new Set();
function getQuestion(theme, level){
  const pool = QUESTIONS[theme.id][level].filter((_,i)=>!used.has(theme.id+level+i));
  if(pool.length){
    const all = QUESTIONS[theme.id][level];
    const idx = all.indexOf(pool[Math.floor(Math.random()*pool.length)]);
    used.add(theme.id+level+idx);
    return all[idx];
  }
  // Question de test (placeholder)
  return {q:`[Question de test] ${theme.name} – niveau ${level}. Tape « test » pour valider.`, answer:'test', placeholder:true};
}

/* ============ ÉTAT ============ */
let players=[], current=0, nb=1, cells=[], remaining=25, timer=null;

/* ============ ACCUEIL ============ */
function legendHTML(){return THEMES.map(t=>`<span class="tag"><span class="dot" style="background:${t.color}"></span>${t.icon} ${t.name}</span>`).join('')}
document.getElementById('legend').innerHTML = legendHTML();
document.getElementById('legend2').innerHTML = legendHTML();
function renderNames(){
  document.getElementById('names').innerHTML = Array.from({length:nb},(_,i)=>
    `<label>JOUEUR ${i+1}</label><input type="text" maxlength="14" value="Joueur ${i+1}" id="pn${i}">`).join('');
}
const nbBtns=document.getElementById('nbBtns');
[1,2,3,4].forEach(n=>{
  const b=document.createElement('button'); b.textContent=n; if(n===1)b.classList.add('sel');
  b.onclick=()=>{nb=n;[...nbBtns.children].forEach(c=>c.classList.remove('sel'));b.classList.add('sel');renderNames()};
  nbBtns.appendChild(b);
});
renderNames();

document.getElementById('start').onclick=()=>{
  players=Array.from({length:nb},(_,i)=>({name:(document.getElementById('pn'+i).value||'Joueur '+(i+1)).trim(),score:0,color:PCOLORS[i]}));
  // Carré latin 5x5 : chaque thème apparaît 1 fois par ligne et par colonne
  cells=[];
  for(let r=0;r<5;r++)for(let c=0;c<5;c++)cells.push({theme:THEMES[(r+2*c)%5],owner:null});
  remaining=25; current=0;
  document.getElementById('setup').classList.add('hidden');
  document.getElementById('game').classList.remove('hidden');
  renderBoard(); renderScores();
};

/* ============ PLATEAU ============ */
function renderBoard(){
  const b=document.getElementById('board'); b.innerHTML='';
  cells.forEach((c,i)=>{
    const d=document.createElement('button');
    d.className='cell'+(c.owner!==null?' done':'');
    d.style.background=c.theme.color;
    d.innerHTML=`${c.theme.icon}<small>${c.theme.name.toUpperCase()}</small>`+
      (c.owner!==null?`<span class="own" style="background:${players[c.owner].color}">${players[c.owner].name[0].toUpperCase()}</span>`:'');
    if(c.owner===null)d.onclick=()=>chooseLevel(i);
    b.appendChild(d);
  });
}
function renderScores(){
  document.getElementById('scores').innerHTML=players.map((p,i)=>
    `<div class="pl ${i===current?'on':''}" style="color:${p.color};border-color:${i===current?p.color:'var(--line)'}"><b>${esc(p.name)}</b> — ${p.score} pts</div>`).join('');
}
function esc(s){return s.replace(/[&<>"]/g,m=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[m]))}

/* ============ MODAL ============ */
const modal=document.getElementById('modal'), box=document.getElementById('box');
function show(html){box.innerHTML=html;modal.classList.remove('hidden')}
function hide(){modal.classList.add('hidden');clearInterval(timer)}

function chooseLevel(i){
  const t=cells[i].theme;
  show(`<span class="theme" style="background:${t.color}">${t.icon} ${t.name.toUpperCase()}</span>
    <div class="q">${esc(players[current].name)}, choisis ton niveau :</div>
    <div class="row diff">
      <button onclick="ask(${i},1)">⭐ Niveau 1<br><small>Facile · 1 pt</small></button>
      <button onclick="ask(${i},2)">⭐⭐ Niveau 2<br><small>Difficile · 2 pts</small></button>
      <button onclick="ask(${i},3)">⭐⭐⭐ Niveau 3<br><small>Très difficile · 3 pts</small></button>
    </div>
    <button style="margin-top:14px" onclick="hide()">Annuler</button>`);
}

/* ============ VÉRIFICATION DE LA RÉPONSE (≥ 80 % de ressemblance) ============ */
const THRESHOLD = 0.8;
function norm(s){
  return String(s).toLowerCase().normalize('NFD').replace(/[̀-ͯ]/g,'')
    .replace(/[^a-z0-9 ]/g,' ').replace(/\s+/g,' ').trim();
}
function similarity(a,b){
  a=norm(a); b=norm(b);
  if(!a&&!b)return 1; if(!a||!b)return 0;
  const m=a.length,n=b.length; let prev=Array.from({length:n+1},(_,j)=>j);
  for(let i=1;i<=m;i++){
    const cur=[i];
    for(let j=1;j<=n;j++)cur[j]=Math.min(prev[j]+1,cur[j-1]+1,prev[j-1]+(a[i-1]===b[j-1]?0:1));
    prev=cur;
  }
  return 1-prev[n]/Math.max(m,n);
}
function isCorrect(input,q){
  return [].concat(q.answer, q.alt||[]).some(r=>similarity(input,r)>=THRESHOLD);
}

function ask(i,level){
  const t=cells[i].theme, q=getQuestion(t,level);
  let left=TIMES[level], done=false; const total=left;
  show(`<span class="theme" style="background:${t.color}">${t.icon} ${t.name.toUpperCase()} · Niveau ${level}</span>
    <div class="bar"><i id="tb"></i></div>
    <div class="q">${esc(q.q)}</div>
    <form id="af"><input type="text" id="ans" autocomplete="off" placeholder="Tape ta réponse ici…">
    <button class="primary" type="submit" style="margin-top:6px">VALIDER</button></form>
    <div class="fb" id="fb"></div>`);
  const inp=document.getElementById('ans'); inp.focus();
  const finish=(text)=>{
    if(done)return; done=true; clearInterval(timer);
    const ok=text!==null && isCorrect(text,q);
    inp.disabled=true; document.querySelector('#af button').classList.add('hidden');
    const fb=document.getElementById('fb');
    if(ok){
      players[current].score+=level; cells[i].owner=current; remaining--;
      fb.style.color='#4cff88'; fb.textContent=`✅ Bonne réponse ! +${level} pt(s), la case est à toi.`;
    }else{
      fb.style.color='#ff6b6b';
      fb.innerHTML=`${text===null?'⏱️ Temps écoulé !':'❌ Raté.'}<br><span style="color:var(--txt);font-weight:400">Réponse attendue : ${esc(String(q.answer))}</span>`;
    }
    const nx=document.createElement('button'); nx.className='primary'; nx.textContent='CONTINUER';
    nx.onclick=()=>{hide(); if(remaining===0){endGame();return} current=(current+1)%players.length; renderBoard(); renderScores()};
    box.appendChild(nx);
    renderBoard(); renderScores();
  };
  document.getElementById('af').onsubmit=e=>{e.preventDefault(); finish(inp.value)};
  timer=setInterval(()=>{left--; document.getElementById('tb').style.width=(left/total*100)+'%'; if(left<=0)finish(null)},1000);
}

/* ============ FIN ============ */
function endGame(){
  document.getElementById('game').classList.add('hidden');
  const e=document.getElementById('end'); e.classList.remove('hidden');
  const sorted=[...players].sort((a,b)=>b.score-a.score);
  const tie=sorted.length>1&&sorted[0].score===sorted[1].score;
  document.getElementById('winner').textContent=tie?'🤝 Égalité !':`🏆 ${sorted[0].name} remporte la partie !`;
  document.getElementById('finalScores').innerHTML=sorted.map(p=>`<p style="color:${p.color};margin:6px 0">${esc(p.name)} — ${p.score} pts</p>`).join('');
}
</script>
</body>
</html>
