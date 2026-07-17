<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cloud Mini Browser</title>
<style>
* { margin:0; padding:0; box-sizing:border-box; }
body { background:#000; color:#fff; font-family:Arial; overflow:hidden; }

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
