<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>Cloud Browser Bing</title>
<style>
* { margin:0; padding:0; box-sizing:border-box; }
body { background:#000; color:#fff; font-family:Arial; overflow:hidden; }

/* THANH URL TO CHO CỤC GẠCH */
.topbar {
  background:#1a1a1a; padding:4px; position:fixed; top:0; width:100%; z-index:100;
  display:flex; gap:3px; height:40px;
}
.topbar input {
  flex:1; padding:6px; font-size:14px; background:#333; color:#fff; border:1px solid #555;
}
.topbar button {
  padding:6px 12px; background:#d32f2f; color:#fff; border:none; font-weight:bold; font-size:14px;
}

/* KHUNG XEM WEB TỶ LỆ 4:3 CHO MÀN HÌNH CỤC GẠCH */
#browser {
  width:100%; height:calc(100vh - 100px); /* trừ topbar + zoom bar */
  border:none; margin-top:40px;
  transform-origin: top left;
}

/* CHỈ CÒN 2 NÚT ZOOM TO */
.zoom-bar {
  position:fixed; bottom:0; width:100%; background:#111; padding:5px;
  display:flex; gap:5px; height:60px;
}
.zoom-bar button {
  flex:1; font-size:24px; font-weight:bold; background:#1976d2; color:#fff; 
  border:2px solid #555; border-radius:8px;
}
.zoom-bar button:active { background:#0d47a1; }
</style>
</head>
<body>

<!-- THANH URL -->
<div class="topbar">
  <input id="url" type="text" placeholder="Dan link hoac tu khoa...">
  <button onclick="go()">GO</button>
</div>

<!-- KHUNG HIỂN THỊ BING -->
<iframe id="browser" src="https://www.bing.com/"></iframe>

<!-- CHỈ CÒN 2 NÚT -->
<div class="zoom-bar">
  <button onclick="zoom(0.9)">- Thu nhỏ</button>
  <button onclick="zoom(1.1)">+ Phóng to</button>
</div>

<script>
let scale = 1;
let iframe = document.getElementById('browser');

function go(){
  let u = document.getElementById('url').value.trim();
  if(!u) return;
  
  // Nếu không có http thì xử lý
  if(!u.startsWith('http')){
    if(u.includes('.')) u = 'https://' + u; // là link
    else u = 'https://www.bing.com/search?q=' + encodeURIComponent(u); // là từ khóa -> tìm Bing
  }
  
  iframe.src = u; // Load trang
} // <-- ĐÃ ĐÓNG HÀM TẠI ĐÂY

function zoom(factor){
  scale *= factor;
  scale = Math.max(0.5, Math.min(3, scale)); // Giới hạn 0.5x - 3x
  iframe.style.transform = 'scale('+scale+')';
}

// TÍNH NĂNG BỔ SUNG CHO CLOUDPHONE + CỤC GẠCH
document.getElementById('url').addEventListener('keypress', function(e){
  if(e.key=='Enter') go(); // Bấm Enter = GO
});

// Phím tắt: 2 = Lên, 8 = Xuống, 4 = Trái, 6 = Phải, 5 = Reset zoom
// * = Thu nhỏ, # = Phóng to. Hợp với phím Nokia/Itel
document.addEventListener('keydown', function(e){
  if(e.key=='2') iframe.contentWindow.scrollBy(0, -150);
  if(e.key=='8') iframe.contentWindow.scrollBy(0, 150);
  if(e.key=='4') iframe.contentWindow.scrollBy(-150, 0);
  if(e.key=='6') iframe.contentWindow.scrollBy(150, 0);
  if(e.key=='5') { scale=1; iframe.style.transform = 'scale(1)'; } // Reset
  if(e.key=='*') zoom(0.9);
  if(e.key=='#') zoom(1.1);
});

// Chặn cuộn 2 lần trên Cloudphone
document.addEventListener('touchmove', function(e){ e.preventDefault(); }, {passive:false});
</script>

</body>
</html>
