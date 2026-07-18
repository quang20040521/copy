
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lite Chat - Cho ĐT Cục Gạch</title>
    <style>
        /* Tối ưu hóa font hệ thống nhẹ nhất có thể */
        body {
            font-family: monospace, sans-serif;
            background-color: #000;
            color: #fff;
            margin: 0;
            padding: 5px;
            font-size: 16px;
        }
        
        /* Tiêu đề gọn gàng, tiết kiệm diện tích màn hình nhỏ */
        .header {
            background-color: #0055ff;
            padding: 5px;
            text-align: center;
            font-weight: bold;
            font-size: 14px;
        }

        /* Danh sách tin nhắn dạng cột dọc dễ cuộn */
        .chat-list {
            list-style: none;
            padding: 0;
            margin: 10px 0;
        }

        .chat-item {
            border-bottom: 1px solid #333;
            padding: 8px 5px;
            display: block;
            text-decoration: none;
            color: #fff;
        }

        /* Highlight khi dùng phím điều hướng trên điện thoại */
        .chat-item:focus, .chat-item:hover {
            background-color: #333;
            color: #00ff00;
            outline: 1px solid #00ff00;
        }

        /* Hộp thoại chat */
        .chat-box {
            height: 200px;
            overflow-y: scroll;
            border: 1px solid #333;
            padding: 5px;
            margin-bottom: 10px;
            background: #111;
        }

        .msg { margin: 5px 0; font-size: 14px; }
        .msg.me { color: #00ff00; }
        .msg.them { color: #00a2ff; }

        /* Các nút bấm to, dễ ấn bằng phím cứng */
        input[type="text"], select {
            width: 95%;
            padding: 8px 2.5%;
            background: #222;
            color: #fff;
            border: 1px solid #555;
            margin-bottom: 5px;
        }

        button {
            width: 100%;
            padding: 10px;
            background: #0055ff;
            color: white;
            border: none;
            font-weight: bold;
            cursor: pointer;
        }
        button:active { background: #0033aa; }

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
        }
    </style>
</head>
<body>

    <div class="header">💬 LITE MESSENGER (CLOUDPHONE)</div>

    <!-- Giao diện 1: Danh sách đoạn chat (Mặc định hiển thị) -->
    <div id="view-list">
        <h3 style="margin: 5px 0; font-size: 14px;">Danh sách bản tin (Ấn số tương ứng):</h3>
        <div class="chat-list">
            <!-- Thêm tiền tố [Phím số] để người dùng cục gạch ấn phím cứng cho nhanh -->
            <a href="#" class="chat-item" onclick="openChat('Mẹ 🏠')" accesskey="1"><span class="key-hint">[1]</span> Mẹ 🏠 <small>(Vừa xong)</small></a>
            <a href="#" class="chat-item" onclick="openChat('Người Yêu ❤️')" accesskey="2"><span class="key-hint">[2]</span> Người Yêu ❤️ <small>(2 phút trước)</small></a>
            <a href="#" class="chat-item" onclick="openChat('Sếp 💼')" accesskey="3"><span class="key-hint">[3]</span> Sếp 💼 <small>(1 giờ trước)</small></a>
            <a href="#" class="chat-item" onclick="openChat('Nhóm Bạn Lớp')" accesskey="4"><span class="key-hint">[4]</span> Nhóm Bạn Lớp <small>(Hôm qua)</small></a>
        </div>
        
        <hr>
        <!-- Tính năng hỗ trợ gửi tin nhanh không cần gõ nhiều -->
        <h3 style="margin: 5px 0; font-size: 14px;">Gửi nhanh SMS/Chat:</h3>
        <select id="quick-contact">
            <option value="Mẹ">Gửi Mẹ</option>
            <option value="Người Yêu">Gửi Người Yêu</option>
        </select>
        <select id="quick-msg">
            <option value="Con đang về ạ!">Con đang về ạ!</option>
            <option value="Đang bận, gọi lại sau nhé.">Đang bận, gọi lại sau.</option>
            <option value="Ok fen!">Ok fen!</option>
        </select>
        <button onclick="sendQuick()">Gửi nhanh [Phím 5]</button>
    </div>

    <!-- Giao diện 2: Hộp thoại Chat bên trong (Mặc định ẩn) -->
    <div id="view-chat" style="display: none;">
        <button onclick="goBack()" style="background:#333; margin-bottom: 5px; padding: 5px;">◀ Trở lại [Phím 0]</button>
        <div id="chat-title" style="font-weight: bold; margin: 5px 0;">Đang chat với...</div>
        
        <div class="chat-box" id="chat-box">
            <!-- Nội dung tin nhắn demo -->
            <div class="msg them"><b>Họ:</b> Alo, đang ở đâu đấy?</div>
            <div class="msg me"><b>Bạn:</b> Đang dùng điện thoại cục gạch chat nè.</div>
        </div>

        <!-- Nhập liệu tối giản -->
        <input type="text" id="input-text" placeholder="Nhập tin nhắn..." autofocus>
        <button onclick="sendMessage()">GỬI 🚀<iframe src="www.bing.com"</iframe></button>
    </div>

    <div class="footer-nav">
        Mẹo: Dùng phím điều hướng để di chuyển. <br>
        Bấm phím số <span class="key-hint">1, 2, 3...</span> để mở nhanh.
    </div>

    <script>
        // Hàm chuyển sang màn hình chat cụ thể
        function openChat(name) {
            document.getElementById('view-list').style.display = 'none';
            document.getElementById('view-chat').style.display = 'block';
            document.getElementById('chat-title').innerText = "💬 Chat với: " + name;
            document.getElementById('input-text').focus();
            
            // Tự động cuộn xuống đáy hộp chat
            var box = document.getElementById('chat-box');
            box.scrollTop = box.scrollHeight;
        }

        // Quay lại danh sách
        function goBack() {
            document.getElementById('view-list').style.style.display = 'block';
            document.getElementById('view-chat').style.display = 'none';
        }

        // Gửi tin nhắn trong hộp thoại
        function sendMessage() {
            var input = document.getElementById('input-text');
            if(input.value.trim() === "") return;
            
            var box = document.getElementById('chat-box');
            box.innerHTML += `<div class="msg me"><b>Bạn:</b> ${input.value}</div>`;
            input.value = "";
            box.scrollTop = box.scrollHeight;
        }

        // Gửi nhanh tin nhắn mẫu
        function sendQuick() {
            var contact = document.getElementById('quick-contact').value;
            var msg = document.getElementById('quick-msg').value;
            alert(`Đã gửi tới [${contact}]: "${msg}" thành công!`);
        }

        // Xử lý phím tắt cứng (Bấm 0 để quay lại khi đang ở phòng chat)
        document.addEventListener('keydown', function(e) {
            if(e.key === '0' && document.getElementById('view-chat').style.display === 'block') {
                goBack();
            }
        });
    </script>
</body>
</html>
