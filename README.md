# copy
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Trình Nghe Nhạc - Liên kết NhacCuaTui</title>
    <style>
        :root {
            --bg-color: #0f172a;
            --card-bg: #1e293b;
            --primary-color: #38bdf8;
            --text-color: #f8fafc;
            --text-secondary: #94a3b8;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            width: 100%;
            max-width: 800px;
            background-color: var(--card-bg);
            border-radius: 16px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
            padding: 24px;
            box-sizing: border-box;
        }

        h1 {
            text-align: center;
            color: var(--primary-color);
            margin-bottom: 20px;
            font-size: 24px;
        }

        /* Khung trình phát nhạc NhacCuaTui */
        .player-wrapper {
            position: relative;
            padding-bottom: 56.25%; /* Tỷ lệ 16:9 cho khung video/player nếu có video */
            height: 180px; /* Cố định chiều cao cho player nhạc */
            overflow: hidden;
            background: #000;
            border-radius: 12px;
            margin-bottom: 25px;
            box-shadow: inset 0 0 20px rgba(0,0,0,0.6);
        }

        .player-wrapper iframe {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            border: none;
        }

        /* Danh sách bài hát */
        .playlist {
            max-height: 400px;
            overflow-y: auto;
            padding-right: 5px;
        }

        /* Tùy chỉnh thanh cuộn */
        .playlist::-webkit-scrollbar {
            width: 6px;
        }
        .playlist::-webkit-scrollbar-thumb {
            background-color: #475569;
            border-radius: 3px;
        }

        .track-item {
            display: flex;
            align-items: center;
            padding: 12px 16px;
            margin-bottom: 8px;
            background-color: rgba(255, 255, 255, 0.03);
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s ease;
            border-left: 4px solid transparent;
        }

        .track-item:hover {
            background-color: rgba(255, 255, 255, 0.08);
            transform: translateX(4px);
        }

        .track-item.active {
            background-color: rgba(56, 189, 248, 0.15);
            border-left-color: var(--primary-color);
        }

        .track-number {
            font-weight: bold;
            color: var(--text-secondary);
            width: 30px;
        }

        .track-info {
            flex-grow: 1;
        }

        .track-title {
            font-weight: 600;
            margin-bottom: 4px;
        }

        .track-artist {
            font-size: 13px;
            color: var(--text-secondary);
        }

        .playing-icon {
            color: var(--primary-color);
            display: none;
        }

        .track-item.active .playing-icon {
            display: block;
        }
        
        .note {
            font-size: 12px;
            color: var(--text-secondary);
            text-align: center;
            margin-top: 15px;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>NCT Music Player</h1>
        
        <!-- Khung Player của NhacCuaTui -->
        <div class="player-wrapper">
             <audio controls>
        <source src="https://nhaccuatui.com/mh/background/"nctId"/type="audio/mpeg">
        Trình duyệt của bạn không hỗ trợ thẻ audio.
    </audio></iframe>
        </div>

        <!-- Danh sách bài hát -->
        <div class="playlist" id="playlist">
            <!-- Dữ liệu bài hát sẽ được tự động đổ vào đây bằng JavaScript -->
        </div>
        
        <div class="note">
            * Mẹo: Bạn có thể lấy mã ID bài hát từ NhacCuaTui điền vào mã nguồn để thêm bài hát yêu thích.
        </div>
    </div>

    <script>
        // Danh sách các bài hát lấy từ NhacCuaTui (Sử dụng ID Player của NhacCuaTui)
        // Cách lấy ID: Vào nhaccuatui, chọn bài hát -> Bấm "Chia sẻ" -> Copy đoạn mã trong phần "Nhúng vào Blog/Web" (Lấy đoạn mã sau /mh/background/XXXXXX)
        const songs = [
            {
                title: "Waiting For You",
                artist: "MONO, Onionn",
                nctId: "L62gXmbyg7bM"
            },
            {
                title: "Chết Trong Em",
                artist: "Thịnh Suy",
                nctId: "8g7V4wZ9Z7V9"
            },
            {
                title: "Tháng Mấy Em Nhớ Anh",
                artist: "Hà Anh Tuấn",
                nctId: "WpYrOnv7nE72"
            },
            {
                title: "Nàng Thơ",
                artist: "Hoàng Dũng",
                nctId: "Ym6SgGg6gZ3O"
            },
            {
                title: "Có Chàng Trai Viết Lên Cây",
                artist: "Phan Mạnh Quỳnh",
                nctId: "T7WvC8bS8Z7t"
            }
        ];

        const playlistContainer = document.getElementById('playlist');
        const playerIframe = document.getElementById('nct-player');

        // Hàm tạo danh sách bài hát giao diện
        function renderPlaylist() {
            playlistContainer.innerHTML = '';
            songs.forEach((song, index) => {
                const trackItem = document.createElement('div');
                trackItem.className = `track-item ${index === 0 ? 'active' : ''}`;
                trackItem.onclick = () => playSong(song.nctId, trackItem);

                trackItem.innerHTML = `
                    <div class="track-number">${index + 1}</div>
                    <div class="track-info">
                        <div class="track-title">${song.title}</div>
                        <div class="track-artist">${song.artist}</div>
                    </div>
                    <div class="playing-icon">♫ Đang phát</div>
                `;
                playlistContainer.appendChild(trackItem);
            });
        }

        // Hàm chuyển bài hát khi người dùng click
        function playSong(nctId, element) {
            // Đổi URL của iframe sang bài hát mới
            playerIframe.src = `https://www.nhaccuatui.com/mh/background/${nctId}`;
            
            // Cập nhật trạng thái Active trên giao diện
            document.querySelectorAll('.track-item').forEach(item => {
                item.classList.remove('active');
            });
            element.classList.add('active');
        }

        // Khởi chạy ứng dụng
        renderPlaylist();
    </script>
</body>
</html>
