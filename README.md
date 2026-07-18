<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lite Browser - Dành Cho ĐT Cục Gạch</title>
    <style>
        /* Tối ưu hóa font hệ thống nhẹ nhất, độ tương phản cao */
        body {
            font-family: monospace, sans-serif;
            background-color: #000;
            color: #fff;
            margin: 0;
            padding: 5px;
            font-size: 16px;
        }
        
        /* Thanh tiêu đề cố định tiết kiệm diện tích */
        .header {
            background-color: #0055ff;
            padding: 5px;
            text-align: center;
            font-weight: bold;
            font-size: 14px;
        }

        /* Danh sách kết quả tìm kiếm dạng cột dọc dễ cuộn */
        .result-list {
            list-style: none;
            padding: 0;
            margin: 10px 0;
        }

        .result-item {
            border-bottom: 1px solid #333;
            padding: 10px 5px;
            display: block;
            text-decoration: none;
            color: #00ff00; /* Màu xanh lá chuẩn retro cho link */
        }

        .result-item title {
            font-weight: bold;
            display: block;
            font-size: 16px;
        }

        .result-item snippet {
            color: #aaa;
            font-size: 13px;
            display: block;
            margin-top: 3px;
        }

        /* Highlight cực mạnh khi dùng phím điều hướng/phím cứng ngắm trúng */
        .result-item:focus, .result-item:hover, .nav-btn:focus {
            background-color: #333;
            color: #ffcc00 !important;
            outline: 2px solid #ffcc00;
        }

        /* Các ô nhập liệu tối giản rộng rãi */
        input[type="text"] {
            width: 94%;
            padding: 10px 3%;
            background: #222;
            color: #fff;
            border: 1px solid #555;
            margin-bottom: 8px;
            font-size: 16px;
        }

        .btn-search {
            width: 100%;
            padding: 12px;
            background: #0055ff;
            color: white;
            border: none;
            font-weight: bold;
            font-size: 16px;
            cursor: pointer;
        }
        .btn-search:active { background: #0033aa; }

        .nav-btn {
            display: inline-block;
            background: #333;
            color: #fff;
            padding: 6px 10px;
            text-decoration: none;
            margin-bottom: 8px;
            font-size: 14px;
            border: 1px solid #555;
        }

        /* Phím tắt hiển thị trực quan số [1], [2]... */
        .key-hint {
            color: #ffcc00;
            font-weight: bold;
            margin-right: 5px;
        }
        
        .footer-nav {
            margin-top: 15px;
            font-size: 12px;
            color: #aaa;
            text-align: center;
            border-top: 1px dashed #444;
            padding-top: 8px;
        }
    </style>
</head>
<body>

    <div class="header">🌐 LITE BROWSER (BING SEARCH)</div>

    <!-- MÀN HÌNH 1: TRANG CHỦ TÌM KIẾM -->
    <div id="view-home">
        <div style="text-align: center; margin: 20px 0 10px 0;">
            <b style="color: #ffcc00; font-size: 20px;">Bing</b> <span style="font-size: 12px; color: #aaa;">Lite</span>
        </div>
        
        <!-- Ô tìm kiếm -->
        <input type="text" id="search-query" placeholder="Nhập từ khóa tìm kiếm..." autofocus>
        <button class="btn-search" onclick="executeSearch()">TÌM KIẾM 🚀</button>

        <!-- Lịch sử / Lối tắt nhanh -->
        <h3 style="margin: 20px 0 5px 0; font-size: 14px;">Truy cập nhanh (Ấn phím số):</h3>
        <div class="result-list">
            <a href="https://m.wikipedia.org" class="result-item" accesskey="1"><span class="key-hint">[1]</span> <title>Wikipedia (Bách khoa toàn thư)</title></a>
            <a href="https://m.dantri.com.vn" class="result-item" accesskey="2"><span class="key-hint">[2]</span> <title>Dân Trí (Tin tức)</title></a>
            <a href="https://m.facebook.com" class="result-item" accesskey="3"><span class="key-hint">[3]</span> <title>Facebook Lite</title></a>
        </div>
    </div>

    <!-- MÀN HÌNH 2: KẾT QUẢ TÌM KIẾM (Mặc định ẩn) -->
    <div id="view-results" style="display: none;">
        <a href="#" class="nav-btn" onclick="goHome()" accesskey="0">◀ Trang chủ [Phím 0]</a>
        
        <div style="font-size: 14px; margin: 5px 0; color: #aaa;">
            Kết quả cho: <b id="keyword-display" style="color: #fff;"></b>
        </div>
        
        <!-- Danh sách kết quả sẽ đổ vào đây -->
        <div class="result-list" id="bing-results">
            <!-- Dữ liệu mẫu hiển thị khi tìm kiếm -->
        </div>
    </div>

    <div class="footer-nav">
        Mẹo: Bấm <span class="key-hint">1, 2, 3</span> để mở nhanh link tương ứng.<br>
        Bấm phím số <span class="key-hint">0</span> để quay lại Trang chủ.
    </div>

    <script>
        // Hàm thực hiện tìm kiếm
        function executeSearch() {
            var query = document.getElementById('search-query').value.trim();
            if (query === "") return;

            // Hiển thị màn hình kết quả
            document.getElementById('view-home').style.display = 'none';
            document.getElementById('view-results').style.display = 'block';
            document.getElementById('keyword-display').innerText = `"${query}"`;

            // Giả lập kết quả trả về từ Bing để hiển thị trên điện thoại cổ (Text-only)
            // Trong thực tế, bạn sẽ dùng Backend để cào (scrape) kết quả từ: 
            // https://www.bing.com/search?q= + encodeURIComponent(query)
            var resultBox = document.getElementById('bing-results');
            
            // Tạo URL chuyển hướng thẳng đến Bing nếu họ muốn xem bản gốc đầy đủ
            var nativeBingUrl = "https://www.bing.com/search?q=" + encodeURIComponent(query);

            resultBox.innerHTML = `
                <a href="${nativeBingUrl}" class="result-item" accesskey="1">
                    <span class="key-hint">[1]</span>
                    <title>Xem kết quả gốc trên Bing.com</title>
                    <snippet>Bấm vào đây để mở trực tiếp trang tìm kiếm Bing chính thức dành cho từ khóa của bạn.</snippet>
                </a>
                <a href="https://vi.wikipedia.org/wiki/${encodeURIComponent(query)}" class="result-item" accesskey="2">
                    <span class="key-hint">[2]</span>
                    <title>${query} - Wikipedia tiếng Việt</title>
                    <snippet>Thông tin bách khoa toàn thư mở về ${query}. Định nghĩa, lịch sử và các bài viết liên quan.</snippet>
                </a>
                <a href="#" class="result-item" accesskey="3" onclick="alert('Tính năng đọc báo qua text đang được phát triển!')">
                    <span class="key-hint">[3]</span>
                    <title>Tin tức mới nhất về ${query}</title>
                    <snippet>Tổng hợp các bài báo, bài viết mới nhất xuất hiện trên mạng xã hội hôm nay.</snippet>
                </a>
            `;
        }

        // Quay lại màn hình chính
        function goHome() {
            document.getElementById('view-home').style.display = 'block';
            document.getElementById('view-results').style.display = 'none';
            document.getElementById('search-query').focus();
        }

        // Bắt sự kiện phím cứng từ bàn phím T9 (Phím số 0)
        document.addEventListener('keydown', function(e) {
            if(e.key === '0' && document.getElementById('view-results').style.display === 'block') {
                goHome();
            }
        });
    </script>
</body>
</html>
