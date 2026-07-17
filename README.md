
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cloud Mini Browser</title>
<style>
* { margin:0; padding:0; box-sizing:border-box; }
body { background:#000; color:#fff; font-family:Arial; overflow:hidden; }

/* THANH ĐIỀU KHIỂN TRÊN */
.topbar {
  background:#1a1a1a; padding:5px; position:fixed; top:0; width:100%; z-index:100;
  display:flex; gap:3px;
}
.topbar input {
  flex:1; padding:8px; font-size:15px; background:#333; color:#fff; border:1px solid #555;
}
.topbar button {
  padding:8px 10px; background:#d32f2f; color:#fff; border:none; font-weight:bold;
}

/* KHUNG XEM WEB */
#browser {
  width:100%; height:100vh; border:none; margin-top:45px; margin-bottom:90px;
}

/* BÀN PHÍM ĐIỀU KHIỂN DƯỚI */
.dpad {
  position:fixed; bottom:0; width:100%; background:#111; padding:5px; text-align:center;
  display:grid; grid-template-columns:1fr 1fr 1fr; gap:3px;
}
.dpad button {
  padding:12px; font-size:18px; background:#333; color:#fff; border:1px solid #555; border-radius:5px;
}
.dpad.wide { grid-column: span 3; background:#444; }
.dpad.nav { background:#1976d2; }
</style>
</head>
<body>

<!-- THANH URL -->
<div class="topbar">
  <button onclick="goBack()">←</button>
  <button onclick="goForward()">→</button>
  <input id="url" type="text" placeholder="Dan link hoac tu khoa...">
  <button onclick="go()">GO</button>
</div>

<!-- KHUNG HIỂN THỊ -->
<iframe id="browser" src="https://www.google.com"></iframe>

<!-- BÀN PHÍM ĐIỀU KHIỂN -->
<div class="dpad">
  <button onclick="scrollY(-200)">↑ Lên</button>
  <button class="nav" onclick="reload()">↺</button>
  <button onclick="scrollY(200)">↓ Xuống</button>

  <button onclick="scrollX(-200)">← Trái</button>
  <button class="nav" onclick="zoom(1)">100%</button>
  <button onclick="scrollX(200)">Phải →</button>

  <button class="wide" onclick="zoom(0.8)">- Thu nhỏ</button>
  <button class="wide" onclick="zoom(1.2)">+ Phóng to</button>
</div>

<script>
let scale = 1;
let history = [];
let historyIndex = -1;
let iframe = document.getElementById('browser');

function go(){
  let u = document.getElementById('url').value.trim();
  if(!u) return;
  if(!u.startsWith('http')){
    if(u.includes('.')) u = 'https://' + u;
    else u = 'https://www.google.com/search?q=' + encodeURIComponent(u);
  }

  // Xử lý Youtube
  if(u.includes('youtube.com/watch?v=')){
    let id = u.split('v=')[1].split('&')[0];
    u = 'https://www.youtube.com/embed/' + id;
  }
  if(u.includes('youtu.be/')){
    let id = u.split('youtu.be/')[1].split('?')[0];
    u = 'https://www.youtube.com/embed/' + id;
  }

  iframe.src = u;
  history.push(u);
  historyIndex = history.length - 1;
}

function goBack(){
  if(historyIndex > 0){
    historyIndex--;
    iframe.src = history[historyIndex];
  }
}

function goForward(){
  if(historyIndex < history.length - 1){
    historyIndex++;
    iframe.src = history[historyIndex];
  }
}

function reload(){
  iframe.src = iframe.src;
}

function scrollX(px){
  iframe.contentWindow.scrollBy(px, 0);
}

function scrollY(px){
  iframe.contentWindow.scrollBy(0, px);
}

function zoom(f){
  if(f==1) scale=1; else scale *= f;
  scale = Math.max(0.5, Math.min(3, scale));
  iframe.style.transform = 'scale('+scale+')';
  iframe.style.transformOrigin = 'top left';
}

// Bấm Enter để GO
document.getElementById('url').addEventListener('keypress', function(e){
  if(e.key=='Enter') go();
});

// Điều khiển bằng phím mũi tên trên cục gạch
document.addEventListener('keydown', function(e){
  if(e.key=='ArrowUp') scrollY(-100);
  if(e.key=='ArrowDown') scrollY(100);
  if(e.key=='ArrowLeft') scrollX(-100);
  if(e.key=='ArrowRight') scrollX(100);
  if(e.key=='*') zoom(0.8);
  if(e.key=='#') zoom(1.2);
});
</script>

</body>
</html>
