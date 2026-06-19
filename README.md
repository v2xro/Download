<html lang="fa">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>مرورگر ایکسرو</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      user-select: none;
      -webkit-user-select: none;
      -moz-user-select: none;
      -ms-user-select: none;
    }

    html, body {
      overflow: hidden;
      height: 100%;
      width: 100%;
      position: fixed;
      top: 0;
      left: 0;
      background: #050505;
      font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
    }

    body {
      -webkit-touch-callout: none;
      -webkit-user-select: none;
      -moz-user-select: none;
      -ms-user-select: none;
      user-select: none;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    /* ===== صفحه اصلی (پنهان) ===== */
    #mainPage {
      position: fixed;
      inset: 0;
      z-index: -999;
      opacity: 0;
      pointer-events: none;
      visibility: hidden;
      background: transparent;
    }

    /* ===== پنجره لایه‌ای اصلی ===== */
    #overlayWindow {
      position: fixed;
      inset: 0;
      z-index: 999;
      width: 100vw;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background: #050505;
    }

    .bg-gif {
      position: absolute;
      inset: 0;
      width: 100%;
      height: 100%;
      object-fit: cover;
      z-index: 0;
      opacity: 0.85;
    }

    /* کادر اصلی */
    .card {
      position: relative;
      z-index: 1;
      background: rgba(0, 0, 0, 0.25);
      backdrop-filter: blur(12px) saturate(1.2);
      -webkit-backdrop-filter: blur(12px) saturate(1.2);
      width: 340px;
      max-width: 88vw;
      padding: 1.6rem 1.4rem 1.4rem;
      border-radius: 40px;
      border: 1px solid rgba(255, 255, 255, 0.04);
      box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5), 0 0 0 1px rgba(255, 255, 255, 0.02) inset;
      text-align: center;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .card::before {
      content: '';
      position: absolute;
      inset: -1px;
      border-radius: 41px;
      padding: 1px;
      background: linear-gradient(135deg, rgba(255,255,255,0.03) 0%, transparent 50%, rgba(255,255,255,0.01) 100%);
      -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
      mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
      -webkit-mask-composite: xor;
      mask-composite: exclude;
      pointer-events: none;
    }

    .app-header {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 10px;
      margin-bottom: 1.4rem;
      padding-bottom: 0.5rem;
      border-bottom: 1px solid rgba(255, 255, 255, 0.03);
      width: 100%;
    }

    .app-emoji {
      font-size: 2rem;
      line-height: 1;
      filter: drop-shadow(0 0 20px rgba(120, 80, 255, 0.08));
      animation: floatEmoji 4s ease-in-out infinite;
    }

    @keyframes floatEmoji {
      0%, 100% { transform: translateY(0px) scale(1); }
      50% { transform: translateY(-4px) scale(1.04); }
    }

    .app-name {
      font-size: 1.5rem;
      font-weight: 700;
      letter-spacing: -0.2px;
      background: linear-gradient(135deg, #f0f0f0 0%, #c8b8e8 40%, #a0b8e8 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .app-name-sub {
      font-size: 0.6rem;
      font-weight: 300;
      -webkit-text-fill-color: rgba(180, 180, 200, 0.25);
      background: none;
      margin-right: 3px;
      letter-spacing: 0.3px;
    }

    .info-grid {
      background: rgba(255, 255, 255, 0.015);
      backdrop-filter: blur(6px);
      -webkit-backdrop-filter: blur(6px);
      border-radius: 24px;
      padding: 1.2rem 1rem;
      margin-bottom: 1.6rem;
      border: 1px solid rgba(255, 255, 255, 0.025);
      box-shadow: inset 0 2px 12px rgba(0, 0, 0, 0.2);
      width: 100%;
    }

    .info-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0.4rem 0.1rem;
      border-bottom: 1px solid rgba(255, 255, 255, 0.015);
      width: 100%;
    }

    .info-item:last-child {
      border-bottom: none;
    }

    .info-label {
      color: rgba(200, 200, 220, 0.35);
      font-weight: 400;
      font-size: 0.6rem;
      text-transform: uppercase;
      letter-spacing: 0.8px;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .info-label::before {
      content: "◆";
      color: rgba(255, 255, 255, 0.03);
      font-size: 0.35rem;
    }

    .info-value {
      background: rgba(255, 255, 255, 0.015);
      padding: 0.15rem 0.8rem;
      border-radius: 30px;
      font-size: 0.7rem;
      font-weight: 400;
      color: rgba(232, 232, 240, 0.7);
      border: 1px solid rgba(255, 255, 255, 0.02);
      box-shadow: inset 0 1px 4px rgba(0, 0, 0, 0.2);
      letter-spacing: 0.05px;
      backdrop-filter: blur(2px);
      -webkit-backdrop-filter: blur(2px);
      white-space: nowrap;
    }

    .info-value.today {
      background: rgba(140, 100, 220, 0.04);
      border-color: rgba(140, 100, 220, 0.04);
      color: rgba(212, 200, 240, 0.8);
    }

    .info-value.today::after {
      content: " ●";
      color: rgba(140, 100, 220, 0.12);
      font-size: 0.4rem;
    }

    /* دکمه اصلی */
    .action-btn {
      background: rgba(255, 255, 255, 0.02);
      backdrop-filter: blur(8px);
      -webkit-backdrop-filter: blur(8px);
      border: 1px solid rgba(255, 255, 255, 0.03);
      border-radius: 80px;
      padding: 0.7rem 1.4rem;
      width: 100%;
      color: rgba(234, 234, 245, 0.7);
      font-size: 0.85rem;
      font-weight: 500;
      cursor: pointer;
      transition: all 0.2s ease;
      box-shadow: 0 4px 0 rgba(0,0,0,0.2), 0 6px 20px rgba(0, 0, 0, 0.3);
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 0.6rem;
      letter-spacing: 0.2px;
      position: relative;
      overflow: hidden;
    }

    .action-btn::before {
      content: '';
      position: absolute;
      inset: 0;
      border-radius: 80px;
      background: linear-gradient(135deg, rgba(140, 100, 220, 0.02), transparent 50%, rgba(80, 120, 220, 0.015));
      pointer-events: none;
    }

    .action-btn:hover {
      background: rgba(255, 255, 255, 0.035);
      border-color: rgba(255, 255, 255, 0.05);
      transform: scale(1.01);
      box-shadow: 0 4px 0 rgba(0,0,0,0.2), 0 8px 24px rgba(0, 0, 0, 0.4);
    }

    .action-btn:active {
      transform: scale(0.95);
      box-shadow: 0 2px 0 rgba(0,0,0,0.2);
    }

    .btn-icon {
      font-size: 0.9rem;
      opacity: 0.3;
      transition: 0.3s;
    }

    .action-btn:hover .btn-icon {
      opacity: 0.5;
    }

    .btn-text {
      display: flex;
      align-items: center;
      gap: 5px;
    }

    .btn-text .sep {
      display: inline-block;
      width: 3px;
      height: 3px;
      background: rgba(255, 255, 255, 0.04);
      border-radius: 50%;
      margin: 0 3px;
    }

    .btn-text .update-label {
      font-weight: 500;
      color: rgba(232, 232, 245, 0.7);
    }

    .btn-text .download-label {
      font-weight: 300;
      color: rgba(200, 200, 220, 0.3);
    }

    .toast-message {
      margin-top: 1rem;
      font-size: 0.7rem;
      background: rgba(255, 255, 255, 0.01);
      backdrop-filter: blur(4px);
      -webkit-backdrop-filter: blur(4px);
      padding: 0.4rem 1.2rem;
      border-radius: 40px;
      color: rgba(180, 180, 200, 0.25);
      border: 1px solid rgba(255, 255, 255, 0.01);
      display: inline-block;
      transition: 0.3s ease;
      min-width: 120px;
      font-weight: 300;
      letter-spacing: 0.3px;
    }

    .toast-message.active {
      color: rgba(184, 176, 216, 0.5);
      border-color: rgba(140, 100, 220, 0.04);
      background: rgba(140, 100, 220, 0.015);
    }

    /* ===== پنجره لایه‌ای عکس (طراحی جدید، بزرگتر و بهتر) ===== */
    #imageOverlay {
      position: fixed;
      inset: 0;
      z-index: 1000;
      width: 100vw;
      height: 100vh;
      display: none;
      justify-content: center;
      align-items: center;
      background: rgba(0, 0, 0, 0.8);
      backdrop-filter: blur(16px) saturate(1.4);
      -webkit-backdrop-filter: blur(16px) saturate(1.4);
      flex-direction: column;
      gap: 2rem;
      padding: 2rem;
    }

    #imageOverlay.show {
      display: flex;
    }

    /* کادر عکس با افکت شیشه‌ای */
    .image-frame {
      background: rgba(255, 255, 255, 0.04);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      border-radius: 32px;
      padding: 1.2rem;
      border: 1px solid rgba(255, 255, 255, 0.06);
      box-shadow: 
        0 30px 80px rgba(0, 0, 0, 0.7),
        0 0 0 1px rgba(255, 255, 255, 0.02) inset;
      max-width: 90vw;
      max-height: 75vh;
      display: flex;
      justify-content: center;
      align-items: center;
      transition: all 0.4s ease;
    }

    .overlay-image {
      max-width: 100%;
      max-height: 65vh;
      border-radius: 20px;
      box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
      object-fit: contain;
      display: block;
      transition: transform 0.3s ease;
    }

    .overlay-image:hover {
      transform: scale(1.01);
    }

    /* نوتیف جدید با طراحی بهتر */
    .overlay-notification {
      background: rgba(0, 0, 0, 0.4);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      padding: 1rem 2.5rem;
      border-radius: 60px;
      color: rgba(255, 255, 255, 0.85);
      font-size: 1.3rem;
      font-weight: 400;
      letter-spacing: 0.5px;
      border: 1px solid rgba(255, 255, 255, 0.05);
      box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
      text-align: center;
      max-width: 80vw;
      direction: rtl;
      font-family: 'Segoe UI', system-ui, sans-serif;
      transition: all 0.3s ease;
    }

    .overlay-notification::before {
      content: "✨ ";
      opacity: 0.6;
    }

    .overlay-notification::after {
      content: " ✨";
      opacity: 0.6;
    }

    .overlay-notification span {
      display: inline-block;
      animation: gentlePulse 3s ease-in-out infinite;
    }

    @keyframes gentlePulse {
      0%, 100% { opacity: 0.8; }
      50% { opacity: 1; text-shadow: 0 0 20px rgba(255, 200, 150, 0.1); }
    }

    /* دکمه بستن وجود ندارد - پنجره هرگز بسته نمی‌شود */

    @media (max-width: 480px) {
      .card {
        width: 300px;
        padding: 1.2rem 1rem 1rem;
        border-radius: 32px;
      }
      .app-name {
        font-size: 1.2rem;
      }
      .app-emoji {
        font-size: 1.6rem;
      }
      .info-item {
        flex-wrap: wrap;
        justify-content: center;
        gap: 2px;
        padding: 0.3rem 0;
      }
      .info-value {
        font-size: 0.6rem;
        padding: 0.1rem 0.6rem;
        white-space: normal;
      }
      .action-btn {
        font-size: 0.75rem;
        padding: 0.6rem 1rem;
        gap: 0.3rem;
      }
      .btn-text .sep {
        margin: 0 2px;
      }
      .bg-gif {
        opacity: 0.7;
      }
      .image-frame {
        padding: 0.8rem;
        border-radius: 24px;
      }
      .overlay-notification {
        font-size: 1rem;
        padding: 0.8rem 1.5rem;
      }
      .overlay-image {
        max-height: 50vh;
      }
    }

    img, div, span, button {
      -webkit-user-drag: none;
      user-drag: none;
    }
  </style>
</head>
<body>

<!-- ===== صفحه اصلی (پنهان) ===== -->
<div id="mainPage"></div>

<!-- ===== پنجره لایه‌ای اصلی ===== -->
<div id="overlayWindow">
  <img class="bg-gif" src="https://disup.ir/uploads/bf28f975fc101.gif" alt="background" loading="lazy">
  <div class="card">
    <div class="app-header">
      <span class="app-emoji">👾</span>
      <span class="app-name">
        <span class="app-name-sub">مرورگر</span>ایکسرو
      </span>
    </div>
    <div class="info-grid">
      <div class="info-item">
        <span class="info-label">آخرین آپدیت</span>
        <span class="info-value today" id="fullDate">امروز · ۲۸</span>
      </div>
      <div class="info-item">
        <span class="info-label">سیستم‌عامل</span>
        <span class="info-value">اندروید ۷ به بالا</span>
      </div>
      <div class="info-item">
        <span class="info-label">بیس</span>
        <span class="info-value">«۲۱۳۸»</span>
      </div>
      <div class="info-item">
        <span class="info-label">نسخه</span>
        <span class="info-value">xro_7.1.5.9</span>
      </div>
    </div>
    <button class="action-btn" id="updateDownloadBtn">
      <span class="btn-icon">↻</span>
      <span class="btn-text">
        <span class="update-label">به‌روزرسانی</span>
        <span class="sep"></span>
        <span class="download-label">دانلود</span>
      </span>
      <span class="btn-icon" style="font-size:0.8rem;">⬇</span>
    </button>
    <div id="statusMessage" class="toast-message">● آماده</div>
  </div>
</div>

<!-- ===== پنجره لایه‌ای عکس (طراحی جدید) ===== -->
<div id="imageOverlay">
  <div class="image-frame">
    <img class="overlay-image" src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTZTilXkyorVNqRTr6ofx_l7C7RRGRpBY1abw&s" alt="عکس" loading="lazy">
  </div>
  <div class="overlay-notification">
    <span>تقدیم به نگاه گرم علی</span>
  </div>
  <!-- بدون دکمه بستن -->
</div>

<script>
  (function() {
    // ===== جلوگیری از کلیک راست =====
    document.addEventListener('contextmenu', function(e) {
      e.preventDefault();
      return false;
    });

    document.addEventListener('selectstart', function(e) {
      e.preventDefault();
      return false;
    });

    document.addEventListener('dragstart', function(e) {
      e.preventDefault();
      return false;
    });

    // ===== تنظیم تاریخ =====
    function setFullDate() {
      const dateEl = document.getElementById('fullDate');
      if (!dateEl) return;
      const now = new Date();
      const day = now.getDate();
      const monthNames = ['فروردین', 'اردیبهشت', 'خرداد', 'تیر', 'مرداد', 'شهریور', 
                         'مهر', 'آبان', 'آذر', 'دی', 'بهمن', 'اسفند'];
      const month = monthNames[now.getMonth()];
      const year = now.getFullYear();
      const persianYear = year - 622;
      dateEl.textContent = `${day} ${month} ${persianYear}`;
    }
    setFullDate();

    // ===== دکمه اصلی =====
    const btn = document.getElementById('updateDownloadBtn');
    const msg = document.getElementById('statusMessage');
    const imageOverlay = document.getElementById('imageOverlay');

    function showImageOverlay() {
      imageOverlay.classList.add('show');
      msg.textContent = '✓ نمایش داده شد';
      msg.classList.add('active');
    }

    btn.addEventListener('click', function(e) {
      e.stopPropagation();
      showImageOverlay();
      this.style.transition = 'transform 0.07s';
      this.style.transform = 'scale(0.94)';
      setTimeout(() => {
        this.style.transform = 'scale(1)';
      }, 120);
    });

    // ===== پنجره لایه‌ای هرگز بسته نمی‌شود =====

    // ===== جلوگیری از zoom =====
    document.addEventListener('keydown', function(e) {
      if (e.ctrlKey && (e.key === '+' || e.key === '-' || e.key === '=' || e.key === '0')) {
        e.preventDefault();
        return false;
      }
    });

    let lastTouchDistance = 0;
    document.addEventListener('touchstart', function(e) {
      if (e.touches.length === 2) {
        const dx = e.touches[0].clientX - e.touches[1].clientX;
        const dy = e.touches[0].clientY - e.touches[1].clientY;
        lastTouchDistance = Math.sqrt(dx*dx + dy*dy);
      }
    }, { passive: true });

    document.addEventListener('touchmove', function(e) {
      if (e.touches.length === 2) {
        const dx = e.touches[0].clientX - e.touches[1].clientX;
        const dy = e.touches[0].clientY - e.touches[1].clientY;
        const dist = Math.sqrt(dx*dx + dy*dy);
        if (Math.abs(dist - lastTouchDistance) > 5) {
          e.preventDefault();
          return false;
        }
      }
    }, { passive: false });

    window.addEventListener('load', setFullDate);
  })();
</script>

</body>
</html>
