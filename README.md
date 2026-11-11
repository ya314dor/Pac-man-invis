<!DOCTYPE html>
<html lang="ru"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Pac-Man: Невидимка</title>
<style>
:root{--bg:#eef0f7;--fg:#16162b;--card:#fff;--mut:#5b5f7a;--acc:#7c5cff;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#0b0b1a;--fg:#f2f2f7;--card:#16162b;--mut:#9a9db8}}
:root[data-theme="dark"]{--bg:#0b0b1a;--fg:#f2f2f7;--card:#16162b;--mut:#9a9db8}
*{box-sizing:border-box}
html,body{height:100%;margin:0}
body{background:var(--bg);color:var(--fg);font:16px/1.3 system-ui,-apple-system,sans-serif;display:flex;justify-content:center;padding:8px;-webkit-user-select:none;user-select:none}
#app{width:100%;max-width:440px;display:flex;flex-direction:column;gap:8px}
#hud{display:flex;justify-content:space-between;font-weight:700;padding:0 4px}
canvas{width:100%;height:auto;background:#05051a;border-radius:12px;touch-action:none;display:block}
#ab{position:relative;overflow:hidden;border:0;border-radius:12px;background:var(--card);color:var(--fg);font:600 16px system-ui,sans-serif;padding:16px;cursor:pointer}
#fill{position:absolute;inset:0 auto 0 0;width:100%;background:var(--acc);opacity:.35;transition:width .1s linear}
#lbl{position:relative}
#hint{color:var(--mut);font-size:13px;text-align:center}
#wrap{position:relative}
.off{display:none!important}
#menu,#shop{position:absolute;inset:0;overflow:auto;border-radius:12px;background:rgba(5,5,26,.94);color:#fff;display:flex;flex-direction:column;align-items:center;justify-content:safe center;gap:8px;padding:12px;text-align:center}
#menu h1,#shop h1{margin:0;font-size:36px;color:#ffe135;letter-spacing:2px}
#menu h2{margin:0 0 6px;font-size:16px;font-weight:600;color:#7ff;letter-spacing:4px}
.mbtn{width:100%;max-width:260px;border:0;border-radius:12px;padding:13px;font:700 17px system-ui,sans-serif;background:#7c5cff;color:#fff;cursor:pointer}
.mbtn.alt{background:#2a2a4a}
#skins{display:flex;gap:12px}
.sk{width:96px;border:2px solid #333a66;border-radius:12px;background:#10102a;color:#fff;padding:8px;font:600 13px system-ui,sans-serif;cursor:pointer}
.sk.sel{border-color:#ffe135}
.sk i,.sk img{display:block;width:44px;height:44px;border-radius:50%;margin:0 auto 6px;object-fit:cover}
.sk i{background:conic-gradient(from 120deg,#ffe135 0 280deg,transparent 280deg)}
#fong{color:#ffe135;font-weight:700;font-size:20px}
.mbtn.hard{background:#c0272d}
.hd{font-weight:700;color:#7ff;margin-top:6px;text-align:left;width:100%}
.sr canvas,.it canvas{width:40px;height:40px;flex:none;border-radius:8px;background:#05051a}
.it>div{flex:1;min-width:0}
.sr{display:flex;align-items:center;gap:10px;padding:6px 8px;border:2px solid #333a66;border-radius:10px;background:#10102a;text-align:left;cursor:pointer;flex:none}
#bk0{position:sticky;top:0;align-self:flex-start;width:auto;padding:10px 16px;z-index:3;flex:none;box-shadow:0 0 0 6px rgba(5,5,26,.94)}
#bk{position:sticky;bottom:0;flex:none;padding:14px;box-shadow:0 0 0 6px rgba(5,5,26,.94)}
.sr.sel{border-color:#ffe135}
#skp{display:flex;align-items:center;gap:10px;width:100%;max-width:260px;background:#10102a;border:2px solid #333a66;border-radius:12px;padding:8px}
#skar{display:flex;flex-direction:column;gap:6px}
.ar{border:0;border-radius:8px;background:#2a2a4a;color:#fff;width:36px;height:34px;font-size:15px;cursor:pointer}
#skv{width:72px;height:108px;border-radius:10px;background:#05051a;touch-action:none;flex:none}
#skt{flex:1;text-align:left;min-width:0}
#skt b{display:block}
#skt small{display:block;color:#9a9db8;font-size:12px;margin-bottom:4px}
.sr small{display:block;color:#9a9db8;font-size:12px}
#skl{display:flex;flex-direction:column;gap:6px;width:100%;max-width:260px;max-height:170px;overflow-y:auto;-webkit-overflow-scrolling:touch}
#best,#how,#hm{color:#9a9db8;font-size:13px}
#mb{border:0;background:var(--card);color:var(--fg);border-radius:8px;padding:4px 10px;font:600 14px system-ui,sans-serif;cursor:pointer}
.it{display:flex;align-items:center;justify-content:space-between;gap:8px;width:100%;background:#10102a;border-radius:10px;padding:8px 10px;text-align:left}
.it small{display:block;color:#9a9db8;font-size:12px}
.mbtn.sm{width:auto;max-width:none;padding:8px 10px;font-size:13px;flex:none}
.mbtn.on{background:#2e9e5b}.mbtn.no{opacity:.45}
#items{display:flex;flex-direction:column;gap:6px;width:100%}
#sf{color:#ffe135;font-weight:700;font-size:18px}
#hint,#how{color:var(--mut);font-size:13px;text-align:center}
#how{margin-top:-4px}
</style></head><body>
<div id="app">
<div id="hud"><span id="sc">Счёт: 0</span><button id="mb">☰ Меню</button><span id="lv">♥♥♥</span></div>
<div id="wrap"><canvas id="c"></canvas>
<div id="menu"><h1>PAC-MAN</h1><h2>НЕВИДИМКА</h2><div id="fong"></div><div id="best"></div>
<button class="mbtn off" id="rs">Продолжить</button>
<button class="mbtn" id="pl">Играть</button>
<button class="mbtn alt" id="hd"></button>
<div id="hm"></div>
<button class="mbtn alt" id="sh">Магазин</button>

<div id="skp"><canvas id="skv" width="144" height="216"></canvas><div id="skt"><b id="skn"></b><small id="sks"></small><button class="mbtn sm off" id="skb">Купить</button><small>Свайп или нажми на соседний скин сверху или снизу</small></div></div>
</div><div id="shop" class="off"><button class="mbtn alt" id="bk0">← Назад</button><h1>Магазин</h1><div id="sf"></div><div id="items"></div><button class="mbtn alt" id="bk">Назад</button></div></div>
<button id="ab"><div id="fill"></div><span id="lbl"></span></button>
<div id="hint">Стрелки / WASD или свайп — движение. Пробел или кнопка — невидимость: 8 с, перезарядка 60 с.</div>
<div id="how">Свайп или стрелки — движение<br>Пробел — невидимость: 8 с, перезарядка 60 с</div>
</div>
<script>
const M=[
"###################",
"#........#........#",
"#o##.###.#.###.##o#",
"#.................#",
"#.##.#.#####.#.##.#",
"#....#...#...#....#",
"####.### # ###.####",
"   #.#       #.#   ",
"####.# ##-## #.####",
"    .  #   #  .    ",
"####.# ##### #.####",
"   #.#       #.#   ",
"####.# ##### #.####",
"#........#........#",
"#.##.###.#.###.##.#",
"#o...............o#",
"#.##.#.#####.#.##.#",
"#....#...#...#....#",
"#.##.##.###.##.##.#",
"#.................#",
"###################"].map(r=>r.padEnd(19,'#').slice(0,19));
const MZ=[M,["###################","#o.......#.......o#","#.###.##...##.###.#","#.................#","#.#.#.##.#.##.#.#.#","#....#...#...#....#","####.### # ###.####","   #.#       #.#   ","####.# ##-## #.####","    .  #   #  .    ","####.# ##### #.####","   #.#       #.#   ","####.# ##### #.####","#........#........#","#.###.##.#.##.###.#","#o...............o#","#.#.#.##.#.##.#.#.#","#.#...#.....#...#.#","#.###.##.#.##.###.#","#.................#","###################"],["###################","#........#........#","#.##.###.#.###.##.#","#o.......#.......o#","#.#.#...###...#.#.#","#....#...#...#....#","####.### # ###.####","   #.#       #.#   ","####.# ##-## #.####","    .  #   #  .    ","####.# ##### #.####","   #.#       #.#   ","####.# ##### #.####","#........#........#","#.##.###.#.###.##.#","#o.......#.......o#","#.#.#.#######.#.#.#","#.................#","#.##.###.#.###.##.#","#.................#","###################"]];
const W=19,H=21,T=20;let INV=8,COOL=60;
const cv=document.getElementById('c');let g=cv.getContext('2d');const S=3;cv.width=W*T*S;cv.height=H*T*S;g.scale(S,S);g.imageSmoothingEnabled=true;g.imageSmoothingQuality='high';
const $=id=>document.getElementById(id);
let grid,dots,score=0,lives=3,state='menu',P,G,want={x:0,y:0},fright=0,invLeft=0,coolLeft=0,hold=0,last=0;
const DIRS=[[1,0],[-1,0],[0,1],[0,-1]];
const wx=x=>(x+W)%W;
function open(x,y){x=wx(x);if(y<0||y>=H)return false;const c=grid[y][x];return c!=='#'&&c!=='-'}
function mk(x,y,sp){return{tx:x,ty:y,dx:0,dy:0,p:0,sp}}
function adv(e,dt,choose){
  let d=e.sp*dt;
  while(d>0){
    if(e.p===0){choose(e);if(!e.dx&&!e.dy)return}
    const s=Math.min(d,1-e.p);e.p+=s;d-=s;
    if(e.p>=0.9999){e.tx=wx(e.tx+e.dx);e.ty+=e.dy;e.p=0}
  }
}
function pChoose(e){
  if((want.x||want.y)&&open(e.tx+want.x,e.ty+want.y)){e.dx=want.x;e.dy=want.y;e.face=Math.atan2(e.dy,e.dx);return}
  if(!open(e.tx+e.dx,e.ty+e.dy)){e.dx=0;e.dy=0}
}
function target(e){
  const px=P.tx,py=P.ty;
  if(e.i===1)return[px+4*P.dx,py+4*P.dy];
  if(e.i===3&&(px-e.tx)**2+(py-e.ty)**2<64)return[1,19];
  return[px,py];
}
function gChoose(e){
  let o=DIRS.filter(d=>open(e.tx+d[0],e.ty+d[1])&&!(d[0]===-e.dx&&d[1]===-e.dy));
  if(!o.length)o=DIRS.filter(d=>open(e.tx+d[0],e.ty+d[1]));
  if(!o.length){e.dx=e.dy=0;return}
  let pick;
  if(!fright&&invLeft<=0&&Math.random()>e.rnd){
    const t=target(e);let b=1e9;
    for(const d of o){const v=(e.tx+d[0]-t[0])**2+(e.ty+d[1]-t[1])**2;if(v<b){b=v;pick=d}}
  }else pick=o[Math.floor(Math.random()*o.length)];
  e.dx=pick[0];e.dy=pick[1];
}
function resetPos(){
  P=mk(9,19,5);P.face=0;want={x:0,y:0};hold=1.2;shield=0;lastKey='';
  const cols=['#ff3b3b','#ff8de0','#35e0ff','#ffa73b'],rn=[0,.15,.3,.45],w=[0,2.5,5,7.5],hx=[9,8,9,10],hy=[7,9,9,9];
  G=cols.map((c,i)=>{const e=mk(hx[i],hy[i],4.4);e.col=c;e.rnd=rn[i];e.i=i;e.wait=w[i];if(i===0)e.dx=-1;return e});
}
function newGame(){
  runHard=hard;runNight=night;INV=hard?10:8;COOL=hard?30:60;grid=MZ[maze].map(r=>r.split(''));dots=0;
  if(runHard){const d=[];grid.forEach((r,y)=>r.forEach((c,x)=>{if(c==='.')d.push([x,y])}));const n=8,st=Math.floor(d.length/n);for(let i=0;i<n;i++){const p=d[Math.floor(st/2)+i*st];if(p)grid[p[1]][p[0]]='o'}}
  for(const r of grid)for(const c of r)if(c==='.'||c==='o')dots++;
  score=0;lives=3;invLeft=0;coolLeft=0;fright=0;resetPos();state='play';
}
function activate(){
  if(state==='play'&&invLeft<=0&&coolLeft<=0)invLeft=INV;
}
function die(){
  lives--;fright=0;
  if(lives<=0){state='over';saveBest()}else resetPos();
}
function update(dt){
  if(invLeft>0){invLeft-=dt;if(invLeft<=0){invLeft=0;coolLeft=COOL}}
  else if(coolLeft>0)coolLeft=Math.max(0,coolLeft-dt);
  if(shield>0)shield=Math.max(0,shield-dt);
  if(hold>0){hold-=dt;return}
  if(fright>0)fright=Math.max(0,fright-dt);
  if(P.p>0&&(want.x||want.y)&&want.x===-P.dx&&want.y===-P.dy){
    P.tx=wx(P.tx+P.dx);P.ty+=P.dy;P.p=1-P.p;P.dx=want.x;P.dy=want.y;P.face=Math.atan2(P.dy,P.dx);
  }
  const ptx=P.tx;adv(P,dt,pChoose);if(Math.abs(P.tx-ptx)>W/2)shield=runHard?1.5:1;
  const px=P.tx+P.dx*P.p,py=P.ty+P.dy*P.p,cx=wx(Math.round(px)),cy=Math.round(py),ch=grid[cy]&&grid[cy][cx];
  if(ch==='.'){grid[cy][cx]=' ';add(10);dots--}
  else if(ch==='o'){grid[cy][cx]=' ';add(50);dots--;fright=runHard?9:6}
  if(dots<=0){state='won';if(runHard){fong+=15000;own.sk_gold=1;if(runNight)own.sk_knight=1;save()}saveBest();return}
  for(const e of G){
    if(e.wait>0){
      e.wait-=dt;
      if(e.wait<=0){e.tx=9;e.ty=7;e.p=0;e.dx=Math.random()<.5?-1:1;e.dy=0}
      continue;
    }
    e.sp=(fright>0?2.8:4.4)*(runHard?1.5:1);
    adv(e,dt,gChoose);
    const gx=e.tx+e.dx*e.p,gy=e.ty+e.dy*e.p;
    if((gx-px)**2+(gy-py)**2<.5){
      if(fright>0){add(200);e.wait=4;e.tx=9;e.ty=9;e.dx=e.dy=0;e.p=0}
      else if(invLeft<=0&&shield<=0){die();return}
    }
  }
}
function gpos(e){return e.wait>0?[(e.tx+.5)*T,(e.ty+.5)*T]:[(e.tx+e.dx*e.p+.5)*T,(e.ty+e.dy*e.p+.5)*T]}
function glow(e,t){
  const[x,y]=gpos(e),fr=fright>0,r=T*.5;
  g.save();g.shadowColor=fr?'#8cf':'#f00';g.shadowBlur=9;g.fillStyle=fr?'#dff':'#ff2a2a';
  for(const s of[-1,1]){g.beginPath();g.ellipse(x+s*r*.38,y-r*.15,1.7,2.3,0,0,7);g.fill()}
  g.restore();
}
function scary(e,t,x,y){
  const fr=fright>0&&e.wait<=0,r=T*.5,w=Math.sin(t*9+e.i*2);
  g.save();g.shadowColor=fr?'#5ae':'#f00';g.shadowBlur=7;
  g.globalAlpha=.82+.18*Math.sin(t*23+e.i*5);
  g.fillStyle=fr?'#9fb4d8':'#c9ccd6';
  g.beginPath();g.moveTo(x-r,y+r);g.lineTo(x-r,y-r*.2);
  g.bezierCurveTo(x-r,y-r*1.35,x+r,y-r*1.35,x+r,y-r*.2);g.lineTo(x+r,y+r);
  for(let k=3;k>=0;k--)g.lineTo(x-r+k*r*.5,y+r*(k%2?.5:1.1)+w*.8*(k%2?1:-1));
  g.closePath();g.fill();g.restore();
  g.fillStyle='#050508';
  for(const s of[-1,1]){g.beginPath();g.ellipse(x+s*r*.38,y-r*.15,2.4,3.2,0,0,7);g.fill()}
  g.beginPath();g.ellipse(x,y+r*.5,r*.42,r*.3+Math.abs(w)*1.2,0,0,7);g.fill();
  g.fillStyle=fr?'#fff':'#e8e8e8';
  for(let k=-1;k<=1;k++){g.beginPath();g.moveTo(x+k*r*.28-1.3,y+r*.28);g.lineTo(x+k*r*.28+1.3,y+r*.28);g.lineTo(x+k*r*.28,y+r*.55);g.fill()}
  glow(e,t);
}
function drawGhost(e,t){
  let x,y;
  if(e.wait>0){x=(e.tx+.5)*T;y=(e.ty+.5)*T}else{x=(e.tx+e.dx*e.p+.5)*T;y=(e.ty+e.dy*e.p+.5)*T}
  if(night){scary(e,t,x,y);return}
  const r=T*.44;
  let c=e.col;
  if(fright>0&&e.wait<=0)c=(fright<2&&Math.floor(t*6)%2)?'#fff':'#2a3cff';
  g.fillStyle=c;g.beginPath();g.arc(x,y-1,r,Math.PI,0);
  g.lineTo(x+r,y+r);g.lineTo(x+r*.5,y+r*.6);g.lineTo(x,y+r);g.lineTo(x-r*.5,y+r*.6);g.lineTo(x-r,y+r);g.closePath();g.fill();
  g.fillStyle='#fff';
  for(const s of[-1,1]){g.beginPath();g.arc(x+s*r*.38,y-3,2.8,0,7);g.fill()}
  g.fillStyle='#112';
  for(const s of[-1,1]){g.beginPath();g.arc(x+s*r*.38+e.dx*1.2,y-3+e.dy*1.2,1.3,0,7);g.fill()}
}
function draw(t){
  g.clearRect(0,0,cv.width,cv.height);
  if(!grid)grid=M.map(r=>r.split(''));
  for(let y=0;y<H;y++)for(let x=0;x<W;x++){
    const c=grid[y][x];
    if(c==='#'){g.fillStyle='#1b2a8a';g.fillRect(x*T+1,y*T+1,T-2,T-2);g.strokeStyle='#4f6bff';g.strokeRect(x*T+3.5,y*T+3.5,T-7,T-7)}
    else if(c==='-'){g.fillStyle='#ff8de0';g.fillRect(x*T,y*T+T/2-2,T,4)}
    else if(c==='.'){g.fillStyle='#ffd9a0';g.beginPath();g.arc((x+.5)*T,(y+.5)*T,2,0,7);g.fill()}
    else if(c==='o'){g.fillStyle='#ffd9a0';g.beginPath();g.arc((x+.5)*T,(y+.5)*T,3+Math.sin(t*8),0,7);g.fill()}
  }
  if(P){
    G.forEach(e=>drawGhost(e,t));
    const x=(P.tx+P.dx*P.p+.5)*T,y=(P.ty+P.dy*P.p+.5)*T,m=((Math.sin(t*14)+1)/2)*.35+.05;
    g.save();
    if(shield>0&&Math.floor(t*12)%2)g.globalAlpha=.2;
    if(invLeft>0){
      g.globalAlpha=.35+.1*Math.sin(t*10);
      g.strokeStyle='#7ff';g.setLineDash([3,3]);g.lineWidth=1.5;
      g.beginPath();g.arc(x,y,T*.62,0,7);g.stroke();g.setLineDash([]);
    }
    if(skin!=='classic'){skins(skin,x,y,t,m)}else{
      g.fillStyle='#ffe135';g.beginPath();g.moveTo(x,y);
      g.arc(x,y,T*.43,P.face+m*Math.PI,P.face+(2-m)*Math.PI);g.closePath();g.fill();
    }
    g.restore();
  }
  if(night&&P&&state!=='menu'){
    const x=(P.tx+P.dx*P.p+.5)*T,y=(P.ty+P.dy*P.p+.5)*T,gr=g.createRadialGradient(x,y,T*(skin==='neon'?2.2:1.5),x,y,T*(skin==='neon'?6.2:4.5));
    gr.addColorStop(0,'rgba(2,2,12,0)');gr.addColorStop(1,'rgba(2,2,12,.97)');g.fillStyle=gr;g.fillRect(0,0,W*T,H*T);
    G.forEach(e=>{if(e.wait<=0)glow(e,t)});
  }
  let msg=null,sub='Коснитесь, чтобы сыграть снова';
  if(state==='over')msg='Игра окончена · '+score;
  else if(state==='won'){msg='Победа! · '+score;if(runHard)sub='HARD пройден: +15000 фонгов и скин!'}
  if(msg){
    g.fillStyle='rgba(5,5,26,.78)';g.fillRect(0,H*T/2-44,W*T,88);
    g.fillStyle='#ffe135';g.textAlign='center';g.font='700 22px system-ui,sans-serif';g.fillText(msg,W*T/2,H*T/2-4);
    g.fillStyle='#fff';g.font='15px system-ui,sans-serif';g.fillText(sub,W*T/2,H*T/2+24);
  }
}
function hud(){
  $('sc').textContent='Счёт '+score+' · '+fong+' '+fw(fong);
  $('lv').textContent='♥'.repeat(Math.max(0,lives));
  let t,f;
  if(invLeft>0){t='Невидим: '+invLeft.toFixed(1)+' с';f=invLeft/INV}
  else if(coolLeft>0){t='Перезарядка: '+Math.ceil(coolLeft)+' с';f=1-coolLeft/COOL}
  else{t='Невидимость готова (Пробел)';f=1}
  $('lbl').textContent=t;$('fill').style.width=(f*100)+'%';
}
function frame(ts){
  const dt=Math.min(.05,(ts-last)/1000||0);last=ts;
  if(state==='play')update(dt);
  draw(ts/1000);hud();if(!$('menu').classList.contains('off'))animPv(ts/1000);requestAnimationFrame(frame);
}
function start(){if(state==='over'||state==='won')newGame()}
const keys={ArrowUp:[0,-1],w:[0,-1],ArrowDown:[0,1],s:[0,1],ArrowLeft:[-1,0],a:[-1,0],ArrowRight:[1,0],d:[1,0]};
addEventListener('keydown',e=>{
  const k=e.key.length===1?e.key.toLowerCase():e.key;
  if(keys[k]){want={x:keys[k][0],y:keys[k][1]};e.preventDefault();if(state!=='play')start()}
  else if(k==='Escape'){if(!$('shop').classList.contains('off'))$('bk').onclick();else openMenu()}
  else if(k===' '||k==='Enter'){e.preventDefault();if(state==='play')activate();else start()}
});
let sx=0,sy=0,moved=false;
cv.addEventListener('pointerdown',e=>{sx=e.clientX;sy=e.clientY;moved=false});
cv.addEventListener('pointermove',e=>{
  if(!e.buttons&&e.pointerType==='mouse')return;
  const dx=e.clientX-sx,dy=e.clientY-sy;
  if(Math.hypot(dx,dy)>18){
    want=Math.abs(dx)>Math.abs(dy)?{x:Math.sign(dx),y:0}:{x:0,y:Math.sign(dy)};
    sx=e.clientX;sy=e.clientY;moved=true;
  }
});
cv.addEventListener('pointerup',()=>{if(!moved&&state!=='play')start()});
$('ab').addEventListener('click',()=>{if(state==='play')activate();else start()});
try{localStorage.removeItem('pm_custom');localStorage.removeItem('pm_photo')}catch(e){}
let skin='classic',best=0,fong=0,maze=0,night=false,own={},hard=false,runHard=false,shield=0,lastKey='',runNight=false;
try{skin=localStorage.getItem('pm_skin')||'classic';best=+localStorage.getItem('pm_best')||0;fong=+localStorage.getItem('pm_fong')||0;maze=+localStorage.getItem('pm_maze')||0;night=localStorage.getItem('pm_night')==='1';hard=localStorage.getItem('pm_hard')==='1';own=JSON.parse(localStorage.getItem('pm_own')||'{}')}catch(e){}
if(maze&&!own['m'+maze])maze=0;
if(skin!=='classic'&&!own['sk_'+skin])skin='classic';
function saveBest(){if(score>best){best=score;try{localStorage.setItem('pm_best',best)}catch(e){}}}
function fw(n){n=Math.abs(n)%100;const d=n%10;return n>10&&n<20?'фонгов':d===1?'фонг':d>1&&d<5?'фонга':'фонгов'}
function add(n){score+=n;fong+=n;try{localStorage.setItem('pm_fong',fong)}catch(e){}}
function setSkin(s){skin=s;try{localStorage.setItem('pm_skin',s)}catch(e){}
  vi=Math.max(0,SK.findIndex(q=>q[0]===s));if(!kd&&Math.abs(cvl)<.15&&kt===null)cp=vi;renderSkl()}
function openMenu(){
  if(state==='play')state='paused';
  $('best').textContent='Рекорд: '+best;$('fong').textContent=fong+' '+fw(fong);
  renderSkl();$('rs').classList.toggle('off',state!=='paused');
  $('menu').classList.remove('off');
}
function closeMenu(){$('menu').classList.add('off')}
const SK=[['classic','Классика','Жёлтый герой',0],['skull','Череп','Челюсть жёт',800],['zombie','Зомби','Глаз вылезает',900],['demon','Демон','Рога, горящие глаза',1200],['clown','Клоун','Жуткая ухмылка',700],['cheese','Сырок','Дырки движутся',500],['smile','Смайлик','Пугается призраков',400],['donut','Пончик','Оставляет крошки',600],['ghostp','Призрак','Прозрачный в невидимости',1500],['fire','Огонь','Оставляет угольки',1000],['neon','Неон','Больше света ночью',2000],['gold','Золотой','Награда за HARD',-1],['knight','Чёрный рыцарь','HARD в режиме «Ночь»',-2]];
function skinRows(bx,buy,pr){
  const h=document.createElement('div');h.className='hd';h.textContent='Скины';bx.append(h);
  SK.forEach(s=>{const id=s[0],p=s[3],ok=p===0||own['sk_'+id],sel=skin===id;
    const r=row(s[1],s[2],sel?'Выбран':ok?'Выбрать':p>0?pr(p):'Закрыт',sel?'on':ok?'':(p>0&&fong>=p?'':'no'),ok?()=>{setSkin(id);save();renderShop()}:p>0?buy('sk_'+id,p,()=>{setSkin(id)}):()=>{});r.prepend(pvCanvas(id));bx.append(r)});
}
let TR=[],lastP=0,PV=[],vi=0,cp=0,cvl=0,kt=null,kd=false,kmov=false;
function pvCanvas(id){const c=document.createElement('canvas');c.width=c.height=88;PV.push({cv:c,id:id});return c}
function pvDraw(cv,id,t){
  const c=cv.getContext('2d'),k=cv.width/(T*2.4);
  c.setTransform(k,0,0,k,0,0);c.clearRect(0,0,T*2.4,T*2.4);
  const sg=g,sP=P,sG=G;g=c;P={face:0,dx:1,dy:0,p:0,tx:0,ty:0};G=[];
  try{
    const x=T*1.2,y=T*1.2,m=((Math.sin(t*14)+1)/2)*.35+.05;
    if(id==='classic')pie(x,y,T*.47,'#ffe135',m);
    else skins(id,x,y,t,m);
  }finally{g=sg;P=sP;G=sG}
}
function skAt(id,x,y,t,m){if(id==='classic')pie(x,y,T*.47,'#ffe135',m);else skins(id,x,y,t,m)}
function skSettle(){const n=SK.length;cp=((Math.round(cp)%n)+n)%n;vi=cp;const s=SK[vi];if((s[3]===0||own['sk_'+s[0]])&&skin!==s[0])setSkin(s[0]);else renderSkl()}
function carDraw(t){
  const cvs=$('skv'),c=cvs.getContext('2d'),k=cvs.width/48,S=30,n=SK.length,dt=Math.min(.05,t-(carDraw.lt||t));carDraw.lt=t;
  if(!kd){
    if(Math.abs(cvl)>.15){cp+=cvl*dt;cvl*=Math.exp(-dt*2.2);kt=null}
    else{cvl=0;const tg=kt!==null?kt:Math.round(cp);cp+=(tg-cp)*Math.min(1,dt*12);
      if(Math.abs(tg-cp)<.003){cp=tg;kt=null;if(kmov){kmov=false;skSettle()}}}
  }
  const ni=((Math.round(cp)%n)+n)%n;if(ni!==vi){vi=ni;renderSkl()}
  c.setTransform(k,0,0,k,0,0);c.clearRect(0,0,48,72);
  const sg=g,sP=P,sG=G;g=c;P={face:0,dx:1,dy:0,p:0,tx:0,ty:0};G=[];
  try{const m=((Math.sin(t*14)+1)/2)*.35+.05,f=Math.floor(cp),it=[f-1,f,f+1,f+2];
    it.sort((a,b)=>Math.abs(b-cp)-Math.abs(a-cp));
    for(const i of it){const q=i-cp;if(Math.abs(q)>1.5)continue;const a=Math.min(1,Math.abs(q));
      c.save();c.globalAlpha=1-.7*a;c.translate(24,36+q*S);c.scale(1-.25*a,1-.25*a);skAt(SK[((i%n)+n)%n][0],0,0,t,m);c.restore()}
  }finally{g=sg;P=sP;G=sG}
}
function animPv(t){carDraw(t);PV=PV.filter(p=>p.cv.isConnected);for(const p of PV)pvDraw(p.cv,p.id,t)}
function skMove(d){kmov=true;cvl=0;kt=(kt!==null?kt:Math.round(cp))+d}
function renderSkl(){
  const s=SK[vi],id=s[0],p=s[3],ok=p===0||own['sk_'+id],b=$('skb');
  $('skn').textContent=s[1];
  $('sks').textContent=skin===id?'Выбран':ok?'Открыт':p>0?'🔒 '+p+' '+fw(p):'🔒 '+s[2];
  b.classList.toggle('off',ok||p<=0);b.classList.toggle('no',fong<p);
}
function renderSklOld(){
  const b=document.createElement('div');
  SK.forEach(s=>{const id=s[0],p=s[3],ok=p===0||own['sk_'+id],sel=skin===id;
    const e=document.createElement('div');e.className='sr'+(sel?' sel':'');
    const n=document.createElement('span'),st=document.createElement('small'),l=document.createElement('div');
    n.textContent=s[1];st.textContent=sel?'Выбран':ok?'Выбрать':p>0?'🔒 '+p+' '+fw(p):'🔒 '+s[2];
    l.append(n,st);e.append(pvCanvas(id),l);
    e.onclick=ok?()=>setSkin(id):()=>{renderShop();$('shop').classList.remove('off')};
    b.append(e)});
}
function disc(x,y,r,c){g.fillStyle=c;g.beginPath();g.arc(x,y,r,0,7);g.fill()}
function clipPie(x,y,r,m){g.beginPath();g.moveTo(x,y);g.arc(x,y,r,P.face+m*Math.PI,P.face+(2-m)*Math.PI);g.closePath();g.clip()}
function pie(x,y,r,c,m){g.fillStyle=c;g.beginPath();g.moveTo(x,y);g.arc(x,y,r,P.face+m*Math.PI,P.face+(2-m)*Math.PI);g.closePath();g.fill()}
function skins(k,x,y,t,m){
  const R=T*.47,ch=(Math.sin(t*14)+1)/2,mv=P.dx||P.dy;
  if((k==='donut'||k==='fire'||(k==='ghostp'&&invLeft>0))&&mv&&state==='play'&&t-lastP>.045){lastP=t;TR.push({x:x,y:y,t0:t,j:Math.random()})}
  TR=TR.filter(p=>t-p.t0<1.1);
  for(const p of TR){const a=(t-p.t0)/1.1;g.save();g.globalAlpha=(1-a)*.9;
    if(k==='fire'){g.fillStyle=a<.4?'#ffd23f':'#ff6a1a';g.fillRect(p.x+(p.j-.5)*8,p.y-a*14,2.5,2.5)}
    else if(k==='donut'){g.fillStyle='#c98a54';g.fillRect(p.x+(p.j-.5)*9,p.y+a*8+(p.j-.5)*6,2,2)}
    else{g.fillStyle='#7ff';g.fillRect(p.x+(p.j-.5)*10,p.y-a*6,2,2)}
    g.restore()}
  switch(k){
  case'skull':{disc(x,y-1,R,'#f2f2ee');g.fillStyle='#111';
    for(const s of[-1,1]){g.beginPath();g.ellipse(x+s*R*.38,y-R*.2,R*.25,R*.3,0,0,7);g.fill()}
    g.beginPath();g.moveTo(x,y+R*.05);g.lineTo(x-R*.1,y+R*.3);g.lineTo(x+R*.1,y+R*.3);g.fill();
    const j=ch*R*.45;g.fillStyle='#e4e4de';g.fillRect(x-R*.55,y+R*.5+j,R*1.1,R*.42);
    g.fillStyle='#111';for(let i=-2;i<=2;i++)g.fillRect(x+i*R*.22-.4,y+R*.5+j,.9,R*.42);break}
  case'zombie':{pie(x,y,R,'#79b956',m);g.save();clipPie(x,y,R,m);disc(x-R*.3,y+R*.3,R*.18,'#4d7a38');
    disc(x+R*.1,y-R*.45,R*.26,'#fff');disc(x+R*.14,y-R*.45,R*.1,'#a11');
    disc(x-R*.45,y-R*.4,R*.18+ch*R*.14,'#fff');disc(x-R*.45,y-R*.4,R*.07,'#a11');g.restore();break}
  case'demon':{g.fillStyle='#8f111a';
    for(const s of[-1,1]){g.beginPath();g.moveTo(x+s*R*.25,y-R*.8);g.lineTo(x+s*R*.75,y-R*1.35);g.lineTo(x+s*R*.85,y-R*.4);g.fill()}
    pie(x,y,R,'#d3202a',m);
    g.save();clipPie(x,y,R,m);g.shadowColor='#fd0';g.shadowBlur=6;g.fillStyle='#ffe14d';
    for(const s of[-1,1]){g.beginPath();g.ellipse(x+s*R*.35,y-R*.35,1.8,1.2,0,0,7);g.fill()}g.restore();break}
  case'clown':{disc(x-R*.95,y-R*.15,R*.4,'#ff7a00');disc(x+R*.95,y-R*.15,R*.4,'#ff7a00');
    disc(x,y,R,'#fdfdfd');disc(x-R*.35,y-R*.3,R*.14,'#111');disc(x+R*.35,y-R*.3,R*.14,'#111');
    disc(x,y+R*.02,R*.22,'#e11');
    g.strokeStyle='#e11';g.lineWidth=2+ch*1.5;g.beginPath();g.arc(x,y+R*.05,R*.62,.2*Math.PI,.8*Math.PI);g.stroke();break}
  case'cheese':{pie(x,y,R,'#ffcf33',m);g.save();clipPie(x,y,R,m);g.fillStyle='#e3a800';
    for(const q of[[-.35,-.25],[.2,.3],[-.1,.45],[.3,-.4]]){g.beginPath();g.arc(x+q[0]*R+ch*1.2,y+q[1]*R-ch*1.2,R*.16,0,7);g.fill()}g.restore();break}
  case'smile':{const sc=fright<=0&&G.some(e=>e.wait<=0&&Math.hypot(gpos(e)[0]-x,gpos(e)[1]-y)<T*3.5);
    disc(x,y,R,'#ffd23f');
    if(sc){disc(x-R*.35,y-R*.25,R*.25,'#fff');disc(x+R*.35,y-R*.25,R*.25,'#fff');disc(x-R*.35,y-R*.25,R*.1,'#111');disc(x+R*.35,y-R*.25,R*.1,'#111');
      g.fillStyle='#111';g.beginPath();g.ellipse(x,y+R*.45,R*.25,R*.32,0,0,7);g.fill();disc(x+R*.8,y-R*.7,R*.14,'#5ad')}
    else{g.fillStyle='#111';for(const s of[-1,1]){g.beginPath();g.ellipse(x+s*R*.35,y-R*.25,R*.12,R*.2,0,0,7);g.fill()}
      g.strokeStyle='#111';g.lineWidth=1.5;g.beginPath();g.arc(x,y+R*.05,R*.55,.15*Math.PI,.85*Math.PI);g.stroke()}break}
  case'donut':{pie(x,y,R,'#d9904f',m);pie(x,y,R*.82,'#ff7eb6',m);disc(x,y,R*.26,'#05051a');
    g.save();clipPie(x,y,R,m);g.fillStyle='#fff';for(const q of[[-.5,-.2],[.2,-.55],[.5,.3],[-.2,.55]])g.fillRect(x+q[0]*R-1,y+q[1]*R-.5,2,1);g.restore();break}
  case'ghostp':{const w=Math.sin(t*9);g.save();g.globalAlpha*=invLeft>0?.55:.95;g.fillStyle='#dfe6f2';
    g.beginPath();g.moveTo(x-R,y+R);g.lineTo(x-R,y-R*.1);g.arc(x,y-R*.1,R,Math.PI,0);g.lineTo(x+R,y+R);
    for(let i=3;i>=0;i--)g.lineTo(x-R+i*R*.5,y+R*(i%2?.55:1.05)+w*.6*(i%2?1:-1));g.closePath();g.fill();g.restore();
    g.fillStyle='#223';for(const s of[-1,1]){g.beginPath();g.ellipse(x+s*R*.36,y-R*.2,R*.15,R*.22,0,0,7);g.fill()}
    g.beginPath();g.ellipse(x,y+R*.35,R*.25,R*.1+ch*R*.25,0,0,7);g.fill();break}
  case'fire':{g.fillStyle='#ff6a1a';
    for(let i=-1;i<=1;i++){const h=R*(.5+.25*Math.sin(t*12+i*2));g.beginPath();g.moveTo(x+i*R*.5-R*.22,y-R*.6);g.lineTo(x+i*R*.5,y-R*.6-h);g.lineTo(x+i*R*.5+R*.22,y-R*.6);g.fill()}
    pie(x,y,R,'#ff6a1a',m);pie(x,y,R*.62,'#ffd23f',m);g.save();clipPie(x,y,R,m);disc(x+R*.1,y-R*.25,1.4,'#411');g.restore();break}
  case'neon':{const c='hsl('+(t*120%360)+',100%,60%)';g.save();g.shadowColor=c;g.shadowBlur=12;g.strokeStyle=c;g.lineWidth=1.6;g.fillStyle='rgba(10,10,34,.85)';
    g.beginPath();g.moveTo(x,y);g.arc(x,y,R,P.face+m*Math.PI,P.face+(2-m)*Math.PI);g.closePath();g.fill();g.stroke();g.restore();break}
  case'gold':{const gr=g.createRadialGradient(x-R*.3,y-R*.3,1,x,y,R);gr.addColorStop(0,'#fff6a0');gr.addColorStop(.5,'#ffd700');gr.addColorStop(1,'#b8860b');
    g.fillStyle=gr;g.beginPath();g.moveTo(x,y);g.arc(x,y,R,P.face+m*Math.PI,P.face+(2-m)*Math.PI);g.closePath();g.fill();
    g.save();g.globalAlpha*=(Math.sin(t*6)+1)/2;g.fillStyle='#fff';g.fillRect(x+R*.5-.5,y-R*.8,1,4);g.fillRect(x+R*.5-2,y-R*.8+1.5,4,1);g.restore();
    g.save();clipPie(x,y,R,m);disc(x+R*.1,y-R*.4,1.4,'#5a3b00');g.restore();break}
  case'knight':{g.fillStyle='#c0272d';g.beginPath();g.ellipse(x,y-R*.95,R*.22,R*.4+ch*2,0,0,7);g.fill();
    pie(x,y,R,'#2b2e38',m);g.strokeStyle='#9aa3b5';g.lineWidth=1;g.beginPath();g.moveTo(x,y);g.arc(x,y,R,P.face+m*Math.PI,P.face+(2-m)*Math.PI);g.closePath();g.stroke();
    g.save();clipPie(x,y,R,m);g.shadowColor='#f22';g.shadowBlur=6;g.fillStyle='#ff3b3b';g.fillRect(x-R*.55,y-R*.28,R*1.1,1.8);g.restore();break}
  }
}
function save(){try{localStorage.setItem('pm_fong',fong);localStorage.setItem('pm_own',JSON.stringify(own));localStorage.setItem('pm_maze',maze);localStorage.setItem('pm_night',night?1:0)}catch(e){}}
function row(n,d,t,c,fn){const e=document.createElement('div');e.className='it';const l=document.createElement('div'),b=document.createElement('b'),s=document.createElement('small');b.textContent=n;s.textContent=d;l.append(b,s);const x=document.createElement('button');x.className='mbtn sm '+c;x.textContent=t;x.onclick=fn;e.append(l,x);return e}
function renderShop(){
  $('sf').textContent=fong+' '+fw(fong);
  const bx=$('items');bx.innerHTML='';
  const buy=(k,p,fn)=>()=>{if(fong<p)return;fong-=p;own[k]=true;fn();save();renderShop()};
  const pr=p=>'Купить · '+p+' '+fw(p);
  bx.append(row('Ночь','Виден только круг света',own.night?(night?'Включена':'Выключена'):pr(1000),own.night?(night?'on':''):(fong<1000?'no':''),own.night?()=>{night=!night;save();renderShop()}:buy('night',1000,()=>{night=true})));
  [['Классика','Обычный лабиринт'],['Решётка','Много перекрёстков'],['Колонны','Узкие проходы']].forEach((m,i)=>{
    const ok=i===0||own['m'+i];
    bx.append(row('Лабиринт «'+m[0]+'»',m[1],maze===i?'Выбран':ok?'Выбрать':pr(1500),maze===i?'on':ok?'':(fong<1500?'no':''),ok?()=>{maze=i;save();renderShop()}:buy('m'+i,1500,()=>{maze=i})));
  });
  skinRows(bx,buy,pr);
}
function setHard(v){hard=v;try{localStorage.setItem('pm_hard',v?1:0)}catch(e){}$('hd').textContent='Режим: '+(v?'HARD':'Обычный');$('hd').classList.toggle('hard',v);$('hm').textContent=v?'Призраки быстрее в 1,5 раза, шарики действуют дольше (9 с). За победу: +15000 фонгов':'';
  const a=v?10:8,c=v?30:60;$('how').innerHTML='Свайп или стрелки — движение<br>Пробел — невидимость: '+a+' с, перезарядка '+c+' с';
  $('hint').textContent='Стрелки / WASD или свайп — движение. Пробел или кнопка — невидимость: '+a+' с, перезарядка '+c+' с.'}
$('hd').onclick=()=>setHard(!hard);
$('skb').onclick=()=>{const s=SK[vi],p=s[3];if(p>0&&fong>=p&&!own['sk_'+s[0]]){fong-=p;own['sk_'+s[0]]=true;setSkin(s[0]);save();$('fong').textContent=fong+' '+fw(fong)}};
const kv=$('skv');let kyy=0,kc0=0,klt=0,kly=0,kt0=0,kw=0,kmv=0;
kv.addEventListener('pointerdown',e=>{kd=true;kmov=true;kt=null;cvl=0;kyy=kly=e.clientY;kc0=cp;klt=kt0=e.timeStamp;kmv=0;try{kv.setPointerCapture(e.pointerId)}catch(x){}});
kv.addEventListener('pointermove',e=>{if(!kd)return;cp=kc0-(e.clientY-kyy)/45;kmv=Math.max(kmv,Math.abs(e.clientY-kyy));const dt=e.timeStamp-klt;if(dt>0){const v=-(e.clientY-kly)/dt*1000/45;cvl=Math.max(-25,Math.min(25,cvl*.6+v*.4))}kly=e.clientY;klt=e.timeStamp});
const kend=e=>{if(!kd)return;kd=false;
  if(kmv<7&&e.timeStamp-kt0<400){cvl=0;const r=kv.getBoundingClientRect(),ry=(e.clientY-r.top)/r.height,b=Math.round(cp);kt=ry<.34?b-1:ry>.66?b+1:b}
  else if(e.timeStamp-klt>90)cvl=0};
kv.addEventListener('pointerup',kend);kv.addEventListener('pointercancel',kend);
kv.addEventListener('wheel',e=>{e.preventDefault();if(e.timeStamp-kw>120){kw=e.timeStamp;skMove(e.deltaY>0?1:-1)}},{passive:false});
$('sh').onclick=()=>{renderShop();$('shop').classList.remove('off')};
$('bk').onclick=()=>{$('shop').classList.add('off');$('fong').textContent=fong+' '+fw(fong)};
$('bk0').onclick=$('bk').onclick;
let sx2=0,sy2=0;$('shop').addEventListener('pointerdown',e=>{sx2=e.clientX;sy2=e.clientY});
$('shop').addEventListener('pointerup',e=>{if(e.clientX-sx2>80&&Math.abs(e.clientY-sy2)<50)$('bk').onclick()});
$('pl').onclick=()=>{newGame();closeMenu()};
$('rs').onclick=()=>{state='play';closeMenu()};

$('mb').onclick=openMenu;
setSkin(skin);setHard(hard);openMenu();
requestAnimationFrame(frame);
</script></body></html>
