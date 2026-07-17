                          
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Trình Xem Link Bất Kỳ</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: Arial, sans-serif; background: #f0f2f5; }
        .header {
            background: #fff; padding: 15px; box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            display: flex; gap: 10px; position: sticky; top: 0; z-index: 10;
        }
        #urlInput {
            flex: 1; padding: 10px 15px; border: 2px solid #ddd; border-radius: 8px; font-size: 16px;
        }
        #urlInput:focus { outline: none; border-color: #1877f2; }
        #goBtn {
            padding: 10px 25px; background: #1877f2; color: #fff; border: none; border-radius: 8px;
            font-size: 16px; font-weight: bold; cursor: pointer;
        }
        #goBtn:hover { background: #0d65d9; }
        #viewer {
            width: 100%; height: calc(100vh - 70px); border: none; background: #fff;
        }
    </style>
</head>
<body>

    
     <div class="header">
  <input id="url" type="text" placeholder="Dan link hoac tu khoa">
  <button onclick="go()">GO</button>
</div>

<iframe id="viewer" src="about:blank"></iframe>

<div class="zoom-bar">
  <button onclick="zoom(0.8)">- Thu nho</button>
  <button onclick="zoom(1)">Reset</button>
  <button onclick="zoom(1.2)">+ Phong to</button>
</div>

<script>
let scale = 1;

function go(){
  let u = document.getElementById('url').value;
  if(!u) return;
  if(!u.startsWith('http')) u = 'https://' + u;

  // Nếu dán link youtube thì tự chuyển sang dạng nhúng
  if(u.includes('youtube.com') || u.includes('youtu.be')){
    let id = u.split('v=')[1]?u.split('v=')[1].split('&')[0]:u.split('/').pop();
    u = 'https://www.youtube.com/embed/' + id;
  }
  // Nếu là chữ thường thì tìm google
  else if(!u.includes('.')){
    u = 'https://www.google.com/search?q=' + encodeURIComponent(u);
  }

  document.getElementById('viewer').src = u;
}

    <div class="container">
        <!-- KHUNG 1: TÌM KIẾM GOOGLE -->
        <div class="box">
            <h2>Hello xem vui vẻ</h2>
            <iframe id="googleFrame" src="https://animevietsub.meme/" style="width:100%; height:600px; border:none; border-radius:8px;"></iframe>
        </div>
    </div>
