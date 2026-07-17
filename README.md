<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cloud Mini Browser</title>
<style>
* { margin:0; padding:0; box-sizing:border-box; }
body { background:#000; color:#fff; font-family:Arial; overflow:hidden; }
.topbar { background:#1a1a1a; padding:5px; position:fixed; top:0; width:100%; z-index:100; display:flex; gap:3px; }
.topbar input { flex:1; padding:8px; font-size:15px; background:#333; color:#fff; border:1px solid #555; }
.topbar button { padding:8px 10px; background:#d32f2f; color:#fff; border:none; font-weight:bold; }
#browser { width:100%; height:100vh; border:none; margin-top:45px; margin-bottom:90px; }
.dpad { position:fixed; bottom:0; width:100%; background:#111; padding:5px; text-align:center; display:grid; grid-template-columns:1fr 1fr 1fr; gap:3px; }
.dpad button { padding:12px; font-size:18px; background:#333; color:#fff; border:1px solid #555; border-radius:5px; }
.dpad.wide { grid-column: span 3; background:#444; }
.dpad.nav { background:#1976d2; }
</style>
</head>
<body>
<div class="topbar">
  <button onclick="goBack()">←</button>
  <button onclick="goForward()">→</button>
  <input id="url" type="text" placeholder="Dan link hoac tu khoa...">
  <button onclick="go()">GO</button>
</div>

<iframe src="https://www.bing.com/search?q=" style="width:100%; height:500px;"></iframe>
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
    else u = 'https://bing.com/?q=' + encodeURIComponent(u); // Đổi ia= thành q=
  }

  iframe.src = u; // Thêm dòng này để load trang
  history.push(u); // Lưu vào lịch sử
  historyIndex = history.length - 1;
} // <-- ĐÃ THÊM DẤU } TẠI ĐÂY

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
document.getElementById('url').addEventListener('keypress', function(e){
  if(e.key=='Enter') go();
});

<!-- 2 NÚT -->
<button id="btnMinus" class="zoom-btn" onclick="zoom(0.9)">-</button>
<button id="btnPlus" class="zoom-btn" onclick="zoom(1.1)">+</button>

<script>
let scale = 1;

function zoom(factor){
  scale *= factor;
  
  // Giới hạn: nhỏ nhất 0.5x, lớn nhất 3x
  if(scale < 0.5) scale = 0.5;
  if(scale > 3) scale = 3;

  document.getElementById('page').style.transform = 'scale(' + scale + ')';
}

// Phím tắt cho cục gạch: * = nhỏ, # = to
document.addEventListener('keydown', function(e){
  if(e.key == '*') zoom(0.9);
  if(e.key == '#') zoom(1.1);
});
</script>

</body>
</html>
