<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <title>Tháng Tư Remix - Final Fix</title>
    <style>
        /* TRONG SUỐT HOÀN TOÀN ĐỂ LỘ VIDEO */
        body, html { 
            margin: 0; padding: 0; height: 100%; 
            overflow: hidden;
            font-family: 'Arial Black', sans-serif;
            background: transparent !important; 
        }

        /* LỚP NỀN CHỜ (CHỈ HIỆN KHI CHƯA CLICK) */
        #waiting-room {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: #000; z-index: 100;
            display: flex; justify-content: center; align-items: center;
            cursor: pointer;
        }

        #waiting-room p {
            color: #fff; font-size: 1.2rem; text-transform: uppercase;
            letter-spacing: 2px; opacity: 0.5;
        }

        /* VIDEO NỀN */
        #video-bg {
            position: fixed;
            top: 50%; left: 50%;
            min-width: 100%; min-height: 100%;
            z-index: -1; 
            transform: translate(-50%, -50%);
            object-fit: cover;
            display: block;
            visibility: hidden; /* Chỉ hiện khi bắt đầu */
        }

        #lyrics-container {
            position: absolute;
            bottom: 12%; 
            width: 100%;
            text-align: center;
            z-index: 10;
            pointer-events: none;
        }

        #lyrics-text {
            color: #fff;
            font-size: 3.5rem;
            font-weight: 900;
            text-transform: uppercase;
            text-shadow: 0 0 15px #fff, 0 0 30px #00ffff, 0 0 45px #00ffff;
            opacity: 0;
            transition: opacity 0.2s ease;
        }

        #lyrics-text.active { opacity: 1; }
    </style>
</head>
<body onclick="startAll()">

    <div id="waiting-room">
        <p>CHẠM VÀO MÀN HÌNH ĐỂ KÍCH HOẠT</p>
    </div>

    <video id="video-bg" muted playsinline>
        <source src="https://raw.githubusercontent.com/nhicode/fps80/main/IMG_1596.MOV" type="video/quicktime">
        <source src="https://raw.githubusercontent.com/nhicode/fps80/main/IMG_1596.MOV" type="video/mp4">
    </video>

    <div id="lyrics-container">
        <div id="lyrics-text"></div>
    </div>

    <audio id="music" src="9919142E-5301-4E5F-A7E7-03B34A407F2D.mp3"></audio>

    <script>
        const video = document.getElementById('video-bg');
        const music = document.getElementById('music');
        const textDisplay = document.getElementById('lyrics-text');
        const waitingRoom = document.getElementById('waiting-room');

        const syncData = [
            { t: 0, txt: "..." },
            { t: 0.1, txt: "Những cánh hoa phai tàn" },
            { t: 2.0, txt: "THẬT NHANH!" },
            { t: 3.5, txt: "EM CÓ VỀ XA..." },
            { t: 5.3, txt: "EM CÓ VỀ XA MÃI?" },
            { t: 7.0, txt: "Tháng tư đôi khi" },
            { t: 9.0, txt: "THẬT MONG MANH" },
            { t: 10.7, txt: "Để mình nói ra" },
            { t: 12.5, txt: "CÂU CHÂN THẬT" },
            { t: 14.5, txt: "Giá như tôi một lần" },
            { t: 16.0, txt: "TIN EM..." },
            { t: 17.7, txt: "Cô tôi thương" },
            { t: 19.7, txt: "nay hoà vào mây gió" },
            { t: 22.5, txt: "Để lại tháng tư..." },
            { t: 24.0, txt: "Ở ĐÓ!" }
        ];

        let isRunning = false;

        function startAll() {
            if (isRunning) return;
            isRunning = true;

            // Xóa bỏ màn hình chờ đen ngay lập tức
            waitingRoom.style.display = 'none';
            
            // Hiện video và kích hoạt âm thanh
            video.style.visibility = 'visible';
            video.play();
            music.play();
            
            function update() {
                const now = music.currentTime;
                const current = syncData.filter(l => l.t <= now).pop();

                if (current && textDisplay.innerText !== current.txt) {
                    textDisplay.classList.remove('active');
                    setTimeout(() => {
                        textDisplay.innerText = current.txt;
                        textDisplay.classList.add('active');
                    }, 30);
                }
                if (!music.paused) requestAnimationFrame(update);
            }
            update();
        }

        // Dừng video ở khung cuối, không reset
        video.onended = () => { video.pause(); };
    </script>
</body>
</html>
