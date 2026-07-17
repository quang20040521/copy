                          
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
        <input type="text" id="searchInput" placeholder="Tìm kiếm Google hoặc dán link Youtube, MP4...">
        <button onclick="handleSearch()">Tìm / Phát</button>
    </div>

    <div class="container">
        <!-- KHUNG 1: TÌM KIẾM GOOGLE -->
        <div class="box">
            <h2>🔍 Tìm kiếm Google</h2>
            <iframe id="googleFrame" src="https://www.google.com" style="width:100%; height:400px; border:none; border-radius:8px;"></iframe>
        </div>
    </div>
