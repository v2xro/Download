
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>پخش کننده ویدیو</title>
    <link href="https://fonts.googleapis.com/css2?family=Lalezar&family=Vazirmatn:wght@400;700;900&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            background-color: #000;
            overflow: hidden;
            height: 100vh;
            width: 100vw;
            font-family: 'Vazirmatn', sans-serif;
        }
        
        .video-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
            transition: opacity 1.5s ease;
        }
        
        video {
            width: 100%;
            height: 100%;
            object-fit: cover;
            background-color: #000;
        }
        
        .controls {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: rgba(0, 0, 0, 0.8);
            transition: all 0.8s ease;
            z-index: 2;
        }
        
        .play-button {
            width: 140px;
            height: 140px;
            background: linear-gradient(135deg, #ff0080, #ff8c00);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.4s ease;
            box-shadow: 0 0 60px rgba(255, 0, 128, 0.5);
            border: 5px solid rgba(255, 255, 255, 0.2);
        }
        
        .play-button:hover {
            transform: scale(1.15);
            box-shadow: 0 0 80px rgba(255, 0, 128, 0.8);
        }
        
        .play-button i {
            font-size: 60px;
            color: white;
            margin-left: 10px;
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
        }
        
        .controls.fade-out {
            opacity: 0;
            visibility: hidden;
        }
        
        /* استایل برای متن نمایشی پس از اتمام ویدیو */
        .text-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            background: linear-gradient(45deg, #000000, #1a0000, #330000);
            z-index: 3;
            opacity: 0;
            visibility: hidden;
            transition: opacity 1.5s ease;
        }
        
        .text-overlay.active {
            opacity: 1;
            visibility: visible;
        }
        
        .main-text {
            font-family: 'Lalezar', cursive;
            font-size: 22vw;
            color: transparent;
            background: linear-gradient(90deg, #ff0000, #ff6600, #ff0000);
            -webkit-background-clip: text;
            background-clip: text;
            text-shadow: 0 0 30px rgba(255, 0, 0, 0.7);
            letter-spacing: 5px;
            text-align: center;
            animation: textGlow 3s infinite alternate;
        }
        
        @keyframes textGlow {
            0% {
                text-shadow: 0 0 20px rgba(255, 0, 0, 0.7);
                transform: scale(1);
            }
            100% {
                text-shadow: 0 0 50px rgba(255, 0, 0, 1), 0 0 80px rgba(255, 102, 0, 0.8);
                transform: scale(1.05);
            }
        }
        
        /* استایل برای متن سازنده */
        .creator-text {
            position: fixed;
            bottom: 20px;
            right: 20px;
            color: rgba(255, 255, 255, 0.4);
            font-size: 14px;
            font-family: 'Vazirmatn', sans-serif;
            z-index: 4;
            direction: rtl;
            text-shadow: 0 1px 3px rgba(0, 0, 0, 0.5);
        }
        
        /* انیمیشن پالس برای دکمه پلی */
        @keyframes pulse {
            0% { 
                box-shadow: 0 0 40px rgba(255, 0, 128, 0.5); 
                transform: scale(1);
            }
            50% { 
                box-shadow: 0 0 80px rgba(255, 0, 128, 0.8); 
                transform: scale(1.05);
            }
            100% { 
                box-shadow: 0 0 40px rgba(255, 0, 128, 0.5); 
                transform: scale(1);
            }
        }
        
        .play-button {
            animation: pulse 2.5s infinite;
        }
        
        /* افکت برای زمانی که ویدیو محو می‌شود */
        .video-container.fade-out {
            opacity: 0;
        }
        
        /* حالت تمام صفحه */
        :fullscreen .creator-text {
            font-size: 16px;
            bottom: 25px;
            right: 25px;
        }
        
        :-webkit-full-screen .creator-text {
            font-size: 16px;
            bottom: 25px;
            right: 25px;
        }
        
        :-moz-full-screen .creator-text {
            font-size: 16px;
            bottom: 25px;
            right: 25px;
        }
        
        /* استایل برای موبایل */
        @media (max-width: 768px) {
            .play-button {
                width: 120px;
                height: 120px;
            }
            
            .play-button i {
                font-size: 50px;
            }
            
            .main-text {
                font-size: 25vw;
            }
            
            .creator-text {
                font-size: 12px;
                bottom: 15px;
                right: 15px;
            }
        }
    </style>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body>
    <!-- ویدیو -->
    <div class="video-container" id="videoContainer">
        <video id="musicVideo" preload="auto">
            <source src="https://uploadkon.ir/uploads/17cf09_25074mhrdad-14040918-164334925.mp4" type="video/mp4">
        </video>
    </div>
    
    <!-- متن نمایشی پس از اتمام ویدیو -->
    <div class="text-overlay" id="textOverlay">
        <div class="main-text" id="mainText">کس ننت</div>
    </div>
    
    <!-- کنترل‌های پخش -->
    <div class="controls" id="videoControls">
        <div class="play-button" id="playButton">
            <i class="fas fa-play"></i>
        </div>
    </div>
    
    <!-- متن سازنده -->
    <div class="creator-text">سازنده سید</div>

    <script>
        // دریافت المان‌های مورد نیاز
        const video = document.getElementById('musicVideo');
        const playButton = document.getElementById('playButton');
        const videoControls = document.getElementById('videoControls');
        const videoContainer = document.getElementById('videoContainer');
        const textOverlay = document.getElementById('textOverlay');
        const mainText = document.getElementById('mainText');
        const body = document.body;
        
        // حالت تمام صفحه به صورت خودکار
        function enterFullscreen() {
            if (!document.fullscreenElement) {
                if (document.documentElement.requestFullscreen) {
                    document.documentElement.requestFullscreen();
                } else if (document.documentElement.mozRequestFullScreen) {
                    document.documentElement.mozRequestFullScreen();
                } else if (document.documentElement.webkitRequestFullscreen) {
                    document.documentElement.webkitRequestFullscreen();
                } else if (document.documentElement.msRequestFullscreen) {
                    document.documentElement.msRequestFullscreen();
                }
            }
        }
        
        // ورود به حالت تمام صفحه پس از کلیک روی دکمه پلی
        playButton.addEventListener('click', function() {
            // ورود به حالت تمام صفحه
            enterFullscreen();
            
            // پخش ویدیو
            video.play();
            
            // اضافه کردن کلاس fade-out برای محو کردن کنترل‌ها
            videoControls.classList.add('fade-out');
            
            // بعد از کامل شدن افکت محو، کنترل‌ها را مخفی می‌کنیم
            setTimeout(() => {
                videoControls.style.display = 'none';
            }, 800);
        });
        
        // رویداد پایان ویدیو
        video.addEventListener('ended', function() {
            // محو کردن ویدیو
            videoContainer.classList.add('fade-out');
            
            // بعد از کامل شدن افکت محو، نمایش متن
            setTimeout(() => {
                textOverlay.classList.add('active');
                
                // پخش مجدد ویدیو پس از 5 ثانیه نمایش متن
                setTimeout(() => {
                    textOverlay.classList.remove('active');
                    videoContainer.classList.remove('fade-out');
                    video.currentTime = 0;
                    video.play();
                }, 5000); // نمایش متن به مدت 5 ثانیه
            }, 1500); // تأخیر 1.5 ثانیه قبل از نمایش متن
        });
        
        // کلیک روی صفحه - ویدیو متوقف نمی‌شود، فقط اگر متوقف بود ادامه می‌یابد
        body.addEventListener('click', function(e) {
            // اگر روی دکمه پلی کلیک شده، کاری نکن (رویداد خود دکمه پلی رسیدگی می‌کند)
            if (e.target.closest('#playButton')) return;
            
            // اگر ویدیو متوقف شده، آن را ادامه بده
            if (video.paused) {
                video.play();
            }
            // اگر ویدیو در حال پخش است، هیچ کاری نکن (متوقف نشود)
        });
        
        // تغییر آیکون دکمه پلی وقتی ویدیو در حال پخش است
        video.addEventListener('play', function() {
            playButton.innerHTML = '<i class="fas fa-pause"></i>';
        });
        
        // تغییر آیکون دکمه پلی وقتی ویدیو مکث می‌شود
        video.addEventListener('pause', function() {
            playButton.innerHTML = '<i class="fas fa-play"></i>';
        });
        
        // تلاش برای ورود به حالت تمام صفحه پس از بارگذاری صفحه
        window.addEventListener('load', function() {
            // نمایش کنترل‌ها و بعد تلاش برای تمام صفحه
            setTimeout(() => {
                // اگر کاربر قبلاً با صفحه تعامل نکرده، دکمه پلی را نشان می‌دهیم
                // اما به صورت خودکار وارد حالت تمام صفحه نمی‌شویم چون مرورگرها اجازه نمی‌دهند
            }, 1000);
        });
        
        // هنگامی که صفحه در حالت تمام صفحه قرار می‌گیرد
        document.addEventListener('fullscreenchange', handleFullscreenChange);
        document.addEventListener('webkitfullscreenchange', handleFullscreenChange);
        document.addEventListener('mozfullscreenchange', handleFullscreenChange);
        document.addEventListener('MSFullscreenChange', handleFullscreenChange);
        
        function handleFullscreenChange() {
            // اگر از حالت تمام صفحه خارج شدیم و ویدیو در حال پخش است، دوباره وارد حالت تمام صفحه شویم
            if (!document.fullscreenElement && 
                !document.webkitFullscreenElement && 
                !document.mozFullScreenElement && 
                !document.msFullscreenElement) {
                
                // اگر ویدیو در حال پخش است، دوباره وارد حالت تمام صفحه شویم
                if (!video.paused) {
                    setTimeout(enterFullscreen, 100);
                }
            }
        }
        
        // جلوگیری از رفتار پیش‌فرض کلیک راست
        document.addEventListener('contextmenu', function(e) {
            e.preventDefault();
            return false;
        });
        
        // پیام کنسول
        console.log('پخش کننده ویدیوی تمام‌صفحه آماده است!');
    </script>
</body>
</html>
