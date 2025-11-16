<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>SIYAEM KOREA Map</title>

  <!-- Kakao Map SDK -->
  <script src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=5bb3be265d3f64faa0ba9d0b6928b2fb"></script>

  <style>
    * {
      box-sizing: border-box;
    }

    html, body {
      margin: 0;
      padding: 0;
      height: 100%;
    }

    body {
      font-family: Arial, sans-serif;
      background: radial-gradient(circle at top, #0f172a 0, #020617 55%, #000 100%);
      color: #e5e7eb;
    }

    .app {
      height: 100vh;
      display: flex;
      flex-direction: column;
      padding: 8px;
      gap: 8px;
    }

    /* ВЕРХНЯЯ ПАНЕЛЬ */
    .top-bar {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 8px 12px;
      border-radius: 999px;
      background: rgba(15, 23, 42, 0.96);
      box-shadow: 0 0 40px rgba(15, 23, 42, 0.7);
    }

    .logo-block {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .logo-dot {
      width: 26px;
      height: 26px;
      border-radius: 999px;
      background: conic-gradient(#f97316, #fb7185, #facc15, #f97316);
      box-shadow: 0 0 18px rgba(248, 250, 252, 0.4);
    }

    .logo-text-main {
      font-weight: 700;
      letter-spacing: 0.12em;
      font-size: 15px;
    }

    .logo-text-sub {
      font-size: 11px;
      color: #9ca3af;
    }

    .top-center {
      flex: 1;
      display: flex;
      justify-content: center;
      font-size: 12px;
      color: #9ca3af;
    }

    .top-actions {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .pill-btn {
      border: none;
      border-radius: 999px;
      padding: 6px 14px;
      font-size: 13px;
      cursor: pointer;
      background: #0f172a;
      color: #e5e7eb;
      display: inline-flex;
      align-items: center;
      gap: 6px;
    }

    .pill-btn.sos {
      background: radial-gradient(circle at top, #ef4444, #b91c1c);
      color: #fee2e2;
      font-weight: 600;
      box-shadow: 0 0 18px rgba(248, 113, 113, 0.7);
    }

    .pill-btn.account {
      background: linear-gradient(to right, #0ea5e9, #6366f1);
      color: #e0f2fe;
      font-weight: 500;
    }

    /* КОНТЕЙНЕР КАРТЫ */
    .map-wrapper {
      flex: 1;
      border-radius: 20px;
      overflow: hidden;
      position: relative;
      background: #020617;
      box-shadow: 0 0 40px rgba(15, 23, 42, 0.85);
    }

    #map {
      width: 100%;
      height: 100%;
    }

    /* КНОПКА ОТКРЫТИЯ ЛЕВОЙ ПАНЕЛИ */
    .floating-toggle {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      z-index: 20;
      border: none;
      cursor: pointer;
      border-radius: 999px;
      padding: 8px 14px;
      font-size: 13px;
      display: flex;
      align-items: center;
      gap: 6px;
      color: #e5e7eb;
      background: rgba(15, 23, 42, 0.94);
      box-shadow: 0 0 22px rgba(15, 23, 42, 0.9);
      backdrop-filter: blur(14px);
    }

    .floating-toggle.left {
      left: 12px;
    }

    .floating-toggle.right {
      right: 12px;
    }

    .floating-toggle span.dot {
      width: 8px;
      height: 8px;
      border-radius: 999px;
      background: #22c55e;
    }

    /* СКРЫВАЮЩИЕСЯ ПАНЕЛИ */
    .side-panel {
      position: absolute;
      top: 0;
      bottom: 0;
      width: 340px;
      max-width: 92vw;
      background: rgba(15, 23, 42, 0.98);
      backdrop-filter: blur(18px);
      box-shadow: 0 0 30px rgba(15, 23, 42, 0.95);
      z-index: 30;
      padding: 14px;
      display: flex;
      flex-direction: column;
      transition: transform 0.25s ease-out;
    }

    .side-panel.left {
      left: 0;
      transform: translateX(-100%);
      border-radius: 0 18px 18px 0;
    }

    .side-panel.right {
      right: 0;
      transform: translateX(100%);
      border-radius: 18px 0 0 18px;
    }

    .side-panel.open.left {
      transform: translateX(0);
    }

    .side-panel.open.right {
      transform: translateX(0);
    }

    .side-panel-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 10px;
    }

    .side-panel-title {
      font-size: 15px;
      font-weight: 600;
    }

    .side-panel-sub {
      font-size: 11px;
      color: #9ca3af;
    }

    .close-btn {
      border-radius: 999px;
      border: none;
      background: #020617;
      color: #9ca3af;
      width: 26px;
      height: 26px;
      cursor: pointer;
      font-size: 14px;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    /* ЛЕВАЯ ПАНЕЛЬ – МЕСТА */
    .filter-row {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
      margin-bottom: 10px;
    }

    .filter-pill {
      padding: 5px 11px;
      font-size: 12px;
      border-radius: 999px;
      border: 1px solid #1f2937;
      background: #020617;
      color: #e5e7eb;
      cursor: pointer;
    }

    .filter-pill.active {
      background: #0ea5e9;
      border-color: #38bdf8;
      color: #e0f2fe;
    }

    .places-list {
      flex: 1;
      overflow-y: auto;
      padding-right: 4px;
      margin-top: 4px;
    }

    .place-item {
      background: #020617;
      border-radius: 12px;
      padding: 10px;
      margin-bottom: 8px;
      cursor: pointer;
      border: 1px solid transparent;
      transition: border-color 0.15s ease, background 0.15s ease;
    }

    .place-item:hover {
      border-color: #38bdf8;
      background: #020617;
    }

    .place-name {
      font-size: 13px;
      font-weight: 600;
    }

    .place-meta {
      font-size: 11px;
      color: #9ca3af;
      margin-top: 2px;
    }

    .place-desc {
      font-size: 11px;
      margin-top: 4px;
      color: #d1d5db;
    }

    /* ПРАВАЯ ПАНЕЛЬ – АККАУНТ */
    .account-body {
      flex: 1;
      display: flex;
      flex-direction: column;
      gap: 10px;
      font-size: 12px;
    }

    .field {
      display: flex;
      flex-direction: column;
      gap: 4px;
    }

    .field label {
      font-size: 11px;
      color: #9ca3af;
    }

    .field input {
      border-radius: 10px;
      border: 1px solid #1f2937;
      padding: 8px 10px;
      font-size: 12px;
      background: #020617;
      color: #e5e7eb;
      outline: none;
    }

    .primary-btn {
      border-radius: 999px;
      border: none;
      padding: 9px 14px;
      font-size: 13px;
      cursor: pointer;
      background: linear-gradient(to right, #22c55e, #16a34a);
      color: #ecfdf5;
      font-weight: 600;
      margin-top: 6px;
    }

    .secondary-text {
      font-size: 11px;
      color: #9ca3af;
      margin-top: 6px;
    }

    .secondary-text b {
      color: #e5e7eb;
    }

    @media (max-width: 768px) {
      .top-bar {
        flex-wrap: wrap;
        gap: 6px;
        border-radius: 16px;
      }
      .top-center {
        display: none;
      }
      .map-wrapper {
        border-radius: 16px;
      }
    }

  </style>
</head>
<body>
<div class="app">
  <!-- ВЕРХНЯЯ ПАНЕЛЬ -->
  <header class="top-bar">
    <div class="logo-block">
      <div class="logo-dot"></div>
      <div>
        <div class="logo-text-main">SIYAEM KOREA</div>
        <div class="logo-text-sub">Карта русскоязычных мест в Корее</div>
      </div>
    </div>

    <div class="top-center">
      Никакой корейской рекламы — только проверенные места
    </div>

    <div class="top-actions">
      <button class="pill-btn sos">
        SOS — экстренная помощь
      </button>
      <button id="accountTopBtn" class="pill-btn account">
        Аккаунт
      </button>
    </div>
  </header>

  <!-- КАРТА + ПАНЕЛИ -->
  <div class="map-wrapper" id="mapWrapper">
    <div id="map"></div>

    <!-- Кнопки-свитчеры -->
    <button id="leftToggle" class="floating-toggle left">
      <span class="dot"></span>
      Места
    </button>

    <button id="rightToggle" class="floating-toggle right">
      Аккаунт
    </button>

    <!-- ЛЕВАЯ ПАНЕЛЬ МЕСТ -->
    <aside id="leftPanel" class="side-panel left">
      <div class="side-panel-header">
        <div>
          <div class="side-panel-title">Места поблизости</div>
          <div class="side-panel-sub">Только проверенные русскоязычные заведения</div>
        </div>
        <button class="close-btn" id="closeLeft">×</button>
      </div>

      <div class="filter-row">
        <button class="filter-pill active">Все</button>
        <button class="filter-pill">Еда</button>
        <button class="filter-pill">Магазины</button>
        <button class="filter-pill">Авто</button>
        <button class="filter-pill">Салоны</button>
        <button class="filter-pill">Развлечения</button>
      </div>

      <div class="places-list" id="placesList">
        <div class="place-item" data-lat="37.55" data-lng="126.99">
          <div class="place-name">Кафе «Сибирь»</div>
          <div class="place-meta">Сеул • Еда</div>
          <div class="place-desc">Домашние пельмени, борщ и компоты в центре Сеула.</div>
        </div>

        <div class="place-item" data-lat="37.54" data-lng="127.02">
          <div class="place-name">Магазин «Балтика»</div>
          <div class="place-meta">Сеул • Магазины</div>
          <div class="place-desc">Русские продукты, сладости, консервы, крупы.</div>
        </div>

        <div class="place-item" data-lat="36.78" data-lng="127.01">
          <div class="place-name">Автосервис «Prestige»</div>
          <div class="place-meta">Асан • Авто</div>
          <div class="place-desc">Полный сервис для автомобиля и русскоязычные мастера.</div>
        </div>

        <div class="place-item" data-lat="37.26" data-lng="127.01">
          <div class="place-name">Салон красоты «Moscow Style»</div>
          <div class="place-meta">Сувон • Салоны</div>
          <div class="place-desc">Стрижки, окрашивание, уход за волосами и бровями.</div>
        </div>

        <div class="place-item" data-lat="35.16" data-lng="129.06">
          <div class="place-name">Кальянная «Doha Lounge»</div>
          <div class="place-meta">Пусан • Развлечения</div>
          <div class="place-desc">Кальян, настольные игры, спортивные трансляции.</div>
        </div>
      </div>
    </aside>

    <!-- ПРАВАЯ ПАНЕЛЬ АККАУНТА -->
    <aside id="rightPanel" class="side-panel right">
      <div class="side-panel-header">
        <div>
          <div class="side-panel-title">Личный кабинет</div>
          <div class="side-panel-sub">Войдите, чтобы управлять избранным и заявками партнёра</div>
        </div>
        <button class="close-btn" id="closeRight">×</button>
      </div>

      <div class="account-body">
        <div class="field">
          <label for="email">Почта или телефон</label>
          <input type="text" id="email" placeholder="example@siyaem.com" />
        </div>

        <div class="field">
          <label for="password">Пароль</label>
          <input type="password" id="password" placeholder="••••••••" />
        </div>

        <button class="primary-btn">Войти</button>

        <div class="secondary-text">
          Нет аккаунта? <b>Регистрация для партнёров и гостей позже здесь.</b>
        </div>

        <div class="secondary-text">
          В будущем отсюда можно будет:
          <br>• Добавлять свои заведения
          <br>• Смотреть статистику по промокодам
          <br>• Управлять SOS-приоритетом и поддержкой
        </div>
      </div>
    </aside>

  </div>
</div>

<script>
  let map;
  let markers = [];

  function initMap() {
    if (!window.kakao || !kakao.maps) {
      console.error("Kakao SDK не загрузился");
      return;
    }

    const container = document.getElementById("map");
    const options = {
      center: new kakao.maps.LatLng(36.5, 127.9),
      level: 12
    };

    map = new kakao.maps.Map(container, options);

    const placesData = [
      { name: "Кафе «Сибирь»", lat: 37.55, lng: 126.99 },
      { name: "Магазин «Балтика»", lat: 37.54, lng: 127.02 },
      { name: "Автосервис «Prestige»", lat: 36.78, lng: 127.01 },
      { name: "Салон красоты «Moscow Style»", lat: 37.26, lng: 127.01 },
      { name: "Кальянная «Doha Lounge»", lat: 35.16, lng: 129.06 }
    ];

    const bounds = new kakao.maps.LatLngBounds();

    placesData.forEach(p => {
      const pos = new kakao.maps.LatLng(p.lat, p.lng);
      const marker = new kakao.maps.Marker({
        map,
        position: pos,
        title: p.name
      });
      markers.push(marker);
      bounds.extend(pos);
    });

    map.setBounds(bounds);
  }

  function setupPanels() {
    const leftPanel = document.getElementById("leftPanel");
    const rightPanel = document.getElementById("rightPanel");
    const leftToggle = document.getElementById("leftToggle");
    const rightToggle = document.getElementById("rightToggle");
    const accountTopBtn = document.getElementById("accountTopBtn");
    const closeLeft = document.getElementById("closeLeft");
    const closeRight = document.getElementById("closeRight");

    function toggleLeft() {
      leftPanel.classList.toggle("open");
      setTimeout(() => { if (map) map.relayout(); }, 260);
    }

    function toggleRight() {
      rightPanel.classList.toggle("open");
      setTimeout(() => { if (map) map.relayout(); }, 260);
    }

    leftToggle.addEventListener("click", toggleLeft);
    rightToggle.addEventListener("click", toggleRight);
    accountTopBtn.addEventListener("click", toggleRight);
    closeLeft.addEventListener("click", toggleLeft);
    closeRight.addEventListener("click", toggleRight);

    // Клик по месту — фокус карты
    const placeItems = document.querySelectorAll(".place-item");
    placeItems.forEach(item => {
      item.addEventListener("click", () => {
        const lat = parseFloat(item.getAttribute("data-lat"));
        const lng = parseFloat(item.getAttribute("data-lng"));
        if (map && !isNaN(lat) && !isNaN(lng)) {
          const pos = new kakao.maps.LatLng(lat, lng);
          map.setLevel(7);
          map.panTo(pos);
        }
      });
    });
  }

  window.onload = function () {
    initMap();
    setupPanels();
  };
</script>

</body>
</html>
