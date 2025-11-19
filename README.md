<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>SIYAEM Korea Map — шаблон</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", system-ui, sans-serif;
      background: #05070b;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      color: #fff;
    }

    /* Рамка телефона */
    .phone {
      width: min(420px, 100vw);
      height: min(900px, 100vh);
      background: #020308;
      border-radius: 36px;
      box-shadow: 0 20px 60px rgba(0,0,0,0.8);
      overflow: hidden;
      border: 1px solid #222;
      position: relative;
    }

    .status-bar {
      height: 22px;
      background: #020308;
      display: flex;
      align-items: center;
      justify-content: flex-end;
      padding: 0 14px;
      font-size: 10px;
      color: #888;
      gap: 8px;
    }

    .app {
      height: calc(100% - 22px);
      display: flex;
      flex-direction: column;
      background: #05070b;
    }

    /* Верхняя панель */
    .top-bar {
      height: 54px;
      padding: 8px 12px;
      display: flex;
      align-items: center;
      gap: 8px;
      background: linear-gradient(to bottom, #070a12ee, #05070bdd);
      border-bottom: 1px solid #151822;
      backdrop-filter: blur(18px);
      z-index: 5;
    }

    .top-logo {
      font-weight: 700;
      font-size: 17px;
      padding: 4px 10px;
      border-radius: 999px;
      background: #05070b;
      color: #f5f5f5;
      border: 1px solid #2b2f3b;
      letter-spacing: 0.05em;
      text-transform: uppercase;
    }

    .top-logo span {
      color: #ffdd55;
    }

    .top-search {
      flex: 1;
      display: flex;
      align-items: center;
      gap: 6px;
      background: rgba(10, 14, 22, 0.95);
      border-radius: 999px;
      padding: 6px 10px;
      border: 1px solid #1c2030;
    }

    .search-icon {
      width: 15px;
      height: 15px;
      border-radius: 50%;
      border: 2px solid #888;
      position: relative;
    }

    .search-icon::after {
      content: "";
      position: absolute;
      width: 7px;
      height: 2px;
      background: #888;
      border-radius: 999px;
      transform: rotate(45deg);
      right: -4px;
      bottom: -1px;
    }

    .search-placeholder {
      font-size: 12px;
      color: #7f8594;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .top-icon-btn {
      width: 32px;
      height: 32px;
      border-radius: 16px;
      border: 1px solid #2a3040;
      background: radial-gradient(circle at 30% 20%, #202534, #090b13);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 16px;
      color: #f5f5f5;
    }

    /* Основная часть */
    .main {
      flex: 1;
      position: relative;
      overflow: hidden;
    }

    /* Экран-карта */
    .screen {
      position: absolute;
      inset: 0;
      display: none;
    }

    .screen.active {
      display: block;
    }

    /* Карта */
    .map-screen {
      background: radial-gradient(circle at 30% 20%, #1e2736, #05070b 60%, #020308);
      color: #fff;
    }

    .map-grid {
      position: absolute;
      inset: 0;
      background-image:
        linear-gradient(to right, rgba(0,0,0,0.6) 1px, transparent 1px),
        linear-gradient(to bottom, rgba(0,0,0,0.6) 1px, transparent 1px);
      background-size: 80px 80px;
      opacity: 0.45;
      pointer-events: none;
    }

    .map-korea-outline {
      position: absolute;
      inset: 18% 10% 18% 18%;
      border-radius: 40% 45% 40% 50%;
      border: 2px solid rgba(130, 184, 255, 0.35);
      box-shadow:
        0 0 25px rgba(140, 190, 255, 0.4),
        0 0 120px rgba(0, 180, 255, 0.15);
      filter: blur(0.2px);
    }

    .city-label {
      position: absolute;
      font-size: 11px;
      color: #c8d5ff;
      text-shadow: 0 0 6px rgba(0,0,0,0.9);
    }

    .city-seoul { top: 29%; left: 45%; }
    .city-busan { bottom: 22%; right: 19%; }
    .city-incheon { top: 26%; left: 38%; }
    .city-daegu { top: 48%; right: 23%; }
    .city-gwangju { bottom: 34%; left: 34%; }

    /* Пины */
    .map-pin {
      position: absolute;
      width: 22px;
      height: 22px;
      border-radius: 999px 999px 999px 0;
      background: #ff5c4d;
      transform: rotate(-45deg);
      box-shadow: 0 4px 10px rgba(0,0,0,0.5);
    }

    .map-pin::after {
      content: "";
      position: absolute;
      inset: 4px;
      border-radius: 50%;
      background: radial-gradient(circle at 30% 20%, #ffe7e3, #ffb4a7);
    }

    .pin-1 { top: 32%; left: 47%; }
    .pin-2 { top: 30%; left: 40%; }
    .pin-3 { top: 55%; right: 24%; }
    .pin-4 { bottom: 32%; left: 32%; }

    /* Панель справа */
    .right-controls {
      position: absolute;
      right: 10px;
      top: 70px;
      display: flex;
      flex-direction: column;
      gap: 10px;
      z-index: 4;
    }

    .ctrl-btn {
      width: 38px;
      height: 38px;
      border-radius: 13px;
      background: radial-gradient(circle at 30% 20%, #262c3d, #090c12);
      border: 1px solid #232838;
      box-shadow: 0 6px 12px rgba(0,0,0,0.7);
      display: flex;
      align-items: center;
      justify-content: center;
      color: #f5f5f5;
      font-size: 18px;
    }

    .ctrl-btn.small {
      font-size: 15px;
    }

    /* Блок топ-мест */
    .top-places-panel {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 70px;
      padding: 0 10px;
      pointer-events: none;
      z-index: 4;
    }

    .top-places-inner {
      background: radial-gradient(circle at 0 0, #262c3d, #0a0d16);
      border-radius: 18px;
      border: 1px solid #242a3c;
      box-shadow: 0 16px 40px rgba(0,0,0,0.9);
      padding: 10px 10px 12px;
      pointer-events: auto;
    }

    .top-places-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 6px;
    }

    .top-places-title {
      font-size: 13px;
      font-weight: 600;
      color: #f5f5f5;
    }

    .top-places-sub {
      font-size: 11px;
      color: #888fa0;
    }

    .top-places-list {
      display: flex;
      overflow-x: auto;
      gap: 8px;
      padding-bottom: 4px;
      margin-top: 6px;
    }

    .top-places-list::-webkit-scrollbar {
      height: 4px;
    }

    .top-places-list::-webkit-scrollbar-thumb {
      background: #2d3344;
      border-radius: 999px;
    }

    .place-card {
      min-width: 170px;
      max-width: 170px;
      background: linear-gradient(145deg, #151927, #10121d);
      border-radius: 14px;
      padding: 8px 9px;
      border: 1px solid #222636;
      display: flex;
      flex-direction: column;
      gap: 3px;
      cursor: pointer;
    }

    .place-card.active {
      border-color: #ffdd55;
      box-shadow: 0 0 12px rgba(255,221,85,0.4);
    }

    .place-name {
      font-size: 12px;
      font-weight: 600;
      color: #f7f7f7;
    }

    .place-meta {
      font-size: 11px;
      color: #a7aec2;
      display: flex;
      justify-content: space-between;
    }

    .place-tag {
      font-size: 10px;
      padding: 3px 7px;
      border-radius: 999px;
      background: rgba(255,221,85,0.08);
      color: #ffdd77;
      border: 1px solid rgba(255,221,85,0.35);
      align-self: flex-start;
      margin-top: 3px;
    }

    .place-distance {
      font-size: 10px;
      color: #8da3ff;
      margin-top: 1px;
    }

    /* Нижняя навигация */
    .bottom-nav {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      height: 60px;
      background: linear-gradient(to top, #020308, #05070b);
      border-top: 1px solid #151824;
      display: flex;
      align-items: center;
      justify-content: space-around;
      z-index: 5;
    }

    .nav-item {
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 2px;
      font-size: 10px;
      color: #7a8190;
      cursor: pointer;
    }

    .nav-icon {
      width: 26px;
      height: 26px;
      border-radius: 10px;
      display: flex;
      align-items: center;
      justify-content: center;
      border: 1px solid transparent;
      font-size: 15px;
    }

    .nav-item.active .nav-icon {
      border-color: #ffdd55;
      background: radial-gradient(circle at 30% 20%, #ffdd55, #b57b1f);
      color: #161616;
      box-shadow: 0 0 18px rgba(255,221,85,0.6);
    }

    .nav-item.active span {
      color: #f5f5f5;
    }

    /* Окно места */
    .place-modal {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 140px;
      padding: 0 12px;
      z-index: 6;
      display: none;
    }

    .place-modal-inner {
      background: radial-gradient(circle at 0 0, #22283a, #0c101b);
      border-radius: 18px;
      border: 1px solid #2a3144;
      box-shadow: 0 20px 50px rgba(0,0,0,0.95);
      padding: 10px 12px 12px;
    }

    .modal-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 10px;
    }

    .modal-title {
      font-size: 14px;
      font-weight: 600;
    }

    .modal-rating {
      font-size: 11px;
      color: #ffdd77;
    }

    .modal-info {
      font-size: 11px;
      color: #a5aec4;
      margin-top: 4px;
    }

    .modal-actions {
      display: flex;
      gap: 6px;
      margin-top: 8px;
    }

    .btn {
      flex: 1;
      font-size: 11px;
      padding: 6px 8px;
      border-radius: 999px;
      border: 1px solid #252c3c;
      background: #10131e;
      color: #f5f5f5;
      text-align: center;
    }

    .btn-main {
      border-color: #ffdd55;
      background: radial-gradient(circle at 30% 20%, #ffdd55, #b57b1f);
      color: #141414;
      font-weight: 600;
    }

    .btn-close {
      max-width: 70px;
      flex: 0 0 auto;
    }

    /* Экран поиска (заглушка) */
    .search-screen {
      padding: 14px 14px 70px;
      background: radial-gradient(circle at 40% 0, #23273a, #05070b 60%, #020308);
    }

    .search-title {
      font-size: 18px;
      font-weight: 600;
      margin-bottom: 10px;
    }

    .search-sub {
      font-size: 12px;
      color: #a0a8bd;
      margin-bottom: 12px;
    }

    .search-chip-row {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
      margin-bottom: 10px;
    }

    .chip {
      font-size: 11px;
      padding: 6px 10px;
      border-radius: 999px;
      border: 1px solid #31384c;
      background: #0d101a;
      color: #c6cbe0;
    }

    .search-input-mock {
      margin-top: 6px;
      padding: 10px 12px;
      border-radius: 16px;
      border: 1px dashed #353c50;
      color: #5f6880;
      font-size: 12px;
    }

    /* Экран аккаунта */
    .account-screen {
      padding: 14px 14px 70px;
      background: radial-gradient(circle at 10% 0, #252b3f, #05070b 55%, #020308);
      overflow-y: auto;
    }

    .account-header {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 12px;
    }

    .avatar {
      width: 42px;
      height: 42px;
      border-radius: 999px;
      border: 2px solid #ffdd55;
      background: radial-gradient(circle at 30% 20%, #ffe7b3, #d39f42);
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: 700;
      color: #141414;
    }

    .account-name {
      font-size: 15px;
      font-weight: 600;
    }

    .account-sub {
      font-size: 11px;
      color: #a3abc0;
    }

    .section-title {
      font-size: 13px;
      font-weight: 600;
      margin: 12px 0 6px;
    }

    .card-row {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }

    .small-card {
      flex: 1 1 calc(50% - 8px);
      min-width: 130px;
      background: linear-gradient(145deg, #151a2a, #0b0e18);
      border-radius: 14px;
      border: 1px solid #292f43;
      padding: 8px 9px;
      font-size: 11px;
      color: #cbd1e6;
    }

    .badge {
      display: inline-block;
      font-size: 10px;
      padding: 3px 7px;
      border-radius: 999px;
      border: 1px solid #414864;
      color: #9aa3c7;
      margin-top: 4px;
    }

    .lang-select {
      margin-top: 4px;
      width: 100%;
      padding: 6px 8px;
      border-radius: 10px;
      border: 1px solid #343a4f;
      background: #070a13;
      color: #e1e5ff;
      font-size: 12px;
    }

    .lang-note {
      font-size: 10px;
      color: #8f97b0;
      margin-top: 3px;
    }

    .list-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 7px 0;
      border-bottom: 1px solid #181d2a;
      font-size: 12px;
      color: #cbd1e6;
    }

    .list-item span:last-child {
      color: #7f88a2;
      font-size: 11px;
    }

    @media (max-height: 720px) {
      .phone {
        border-radius: 24px;
      }
    }
  </style>
</head>
<body>
<div class="phone">
  <div class="status-bar">
    <span>13:58</span>
    <span>LTE</span>
    <span>🔋 76%</span>
  </div>

  <div class="app">
    <!-- Верх -->
    <div class="top-bar">
      <div class="top-logo"><span>SIYAEM</span> KOREA</div>
      <div class="top-search">
        <div class="search-icon"></div>
        <div class="search-placeholder">Поиск мест, самушилей, кафе…</div>
      </div>
      <div class="top-icon-btn">☰</div>
    </div>

    <!-- Основное содержимое -->
    <div class="main">
      <!-- Экран карты -->
      <div class="screen map-screen active" id="screen-map">
        <div class="map-grid"></div>
        <div class="map-korea-outline"></div>

        <div class="city-label city-seoul">Сеул</div>
        <div class="city-label city-incheon">Инчхон</div>
        <div class="city-label city-daegu">Тэгу</div>
        <div class="city-label city-busan">Пусан</div>
        <div class="city-label city-gwangju">Кванджу</div>

        <div class="map-pin pin-1"></div>
        <div class="map-pin pin-2"></div>
        <div class="map-pin pin-3"></div>
        <div class="map-pin pin-4"></div>

        <div class="right-controls">
          <div class="ctrl-btn small">⤢</div>
          <div class="ctrl-btn">◎</div>
          <div class="ctrl-btn">＋</div>
          <div class="ctrl-btn small">➤</div>
        </div>

        <!-- Топ-10 мест рядом -->
        <div class="top-places-panel">
          <div class="top-places-inner">
            <div class="top-places-header">
              <div>
                <div class="top-places-title">Топ-10 мест рядом</div>
                <div class="top-places-sub">По оценкам иностранцев в Корее</div>
              </div>
              <div class="top-places-sub">1.2 км · Сеул</div>
            </div>

            <div class="top-places-list" id="places-list">
              <!-- карточки заполняются скриптом -->
            </div>
          </div>
        </div>

        <!-- Модальное окно места -->
        <div class="place-modal" id="place-modal">
          <div class="place-modal-inner">
            <div class="modal-row">
              <div>
                <div class="modal-title" id="modal-title">Кафе “Han River View”</div>
                <div class="modal-rating" id="modal-rating">★ 4.9 · кофе, десерты</div>
              </div>
              <button class="btn btn-close" id="modal-close">✕</button>
            </div>
            <div class="modal-info" id="modal-info">
              350 м · 5 мин пешком · Русскоязычный персонал, скидка по купону SIYAEM.
            </div>
            <div class="modal-actions">
              <div class="btn">Добавить в избранное</div>
              <div class="btn btn-main">Открыть как будто на карте</div>
            </div>
          </div>
        </div>
      </div>

      <!-- Экран поиска -->
      <div class="screen search-screen" id="screen-search">
        <div class="search-title">Поиск</div>
        <div class="search-sub">Макет: здесь будет поиск по самушилям, кафе, авто, жилью и т.д.</div>

        <div class="search-chip-row">
          <div class="chip">Самушили рядом</div>
          <div class="chip">Кафе с русским меню</div>
          <div class="chip">Работа на сегодня</div>
          <div class="chip">Автосервисы</div>
          <div class="chip">Салоны красоты</div>
        </div>

        <div class="search-input-mock">
          Здесь будет строка поиска и фильтры. Пока это просто шаблон для презентации.
        </div>
      </div>

      <!-- Экран аккаунта -->
      <div class="screen account-screen" id="screen-account">
        <div class="account-header">
          <div class="avatar">IG</div>
          <div>
            <div class="account-name">Мой аккаунт</div>
            <div class="account-sub">Купоны, избранное, язык, поддержка</div>
          </div>
        </div>

        <div class="section-title">Купоны и бонусы</div>
        <div class="card-row">
          <div class="small-card">
            До <b>–10%</b> в партнёрских кафе.<br>
            Показывай карту SIYAEM при оплате.
            <div class="badge">Активно</div>
          </div>
          <div class="small-card">
            Бесплатный трансфер на самушиль<br>
            (1 раз в месяц).
            <div class="badge">Скоро</div>
          </div>
        </div>

        <div class="section-title">Избранное</div>
        <div class="small-card">
          12 мест в избранном: кафе, самушили, салоны, авто.<br>
          Просто макет — нажатия никуда не ведут.
          <div class="badge">Только демонстрация</div>
        </div>

        <div class="section-title">Язык приложения</div>
        <select class="lang-select">
          <option>Русский</option>
          <option>Қазақ тілі (Казахстан)</option>
          <option>Українська</option>
          <option>Oʻzbekcha</option>
          <option>한국어 (Korean)</option>
          <option>English</option>
          <option>Беларуская</option>
          <option>Кыргызча</option>
          <option>Тоҷикӣ</option>
          <option>Azərbaycan dili</option>
          <option>Հայերեն</option>
          <option>ქართული</option>
        </select>
        <div class="lang-note">
          Для презентации: языки просто отображаются, переключение пока не реализовано.
        </div>

        <div class="section-title">Настройки и помощь</div>
        <div class="list-item">
          <span>Поддержка 24/7</span><span>Чат / WhatsApp</span>
        </div>
        <div class="list-item">
          <span>О проекте SIYAEM Korea</span><span>Описание</span>
        </div>
        <div class="list-item">
          <span>Партнёрам</span><span>Условия</span>
        </div>
        <div class="list-item">
          <span>Выход</span><span>Только визуально</span>
        </div>
      </div>

      <!-- Нижнее меню -->
      <div class="bottom-nav">
        <div class="nav-item active" data-screen="map">
          <div class="nav-icon">🗺️</div>
          <span>Карта</span>
        </div>
        <div class="nav-item" data-screen="search">
          <div class="nav-icon">🔍</div>
          <span>Поиск</span>
        </div>
        <div class="nav-item" data-screen="account">
          <div class="nav-icon">👤</div>
          <span>Аккаунт</span>
        </div>
      </div>
    </div>
  </div>
</div>

<script>
  // Данные 10 мест рядом
  const places = [
    { name: "Кафе Han River View", type: "кафе", rating: 4.9, distance: "350 м", tag: "Кофе, десерты" },
    { name: "Samushil Asan Line 3", type: "самушиль", rating: 4.7, distance: "1.1 км", tag: "Дневная смена" },
    { name: "Русский маркет Seoul", type: "маркет", rating: 4.8, distance: "900 м", tag: "Продукты СНГ" },
    { name: "K-Garage Service", type: "автосервис", rating: 4.6, distance: "2.0 км", tag: "Диагностика" },
    { name: "Beauty Room K-Style", type: "салон", rating: 4.9, distance: "1.4 км", tag: "Парикмахерская" },
    { name: "24/7 Help Point", type: "центр помощи", rating: 4.8, distance: "650 м", tag: "Перевод, документы" },
    { name: "Samushil Night Shift", type: "самушиль", rating: 4.5, distance: "3.2 км", tag: "Ночная смена" },
    { name: "Russian Bakery Seoul", type: "кафе", rating: 4.9, distance: "1.9 км", tag: "Выпечка, кофе" },
    { name: "Car Rent Korea", type: "авто", rating: 4.4, distance: "2.5 км", tag: "Аренда" },
    { name: "Language Hub", type: "центр", rating: 4.7, distance: "800 м", tag: "Курсы корейского" }
  ];

  const placesList = document.getElementById("places-list");
  const modal = document.getElementById("place-modal");
  const modalTitle = document.getElementById("modal-title");
  const modalRating = document.getElementById("modal-rating");
  const modalInfo = document.getElementById("modal-info");
  const modalClose = document.getElementById("modal-close");

  // Рисуем карточки
  places.forEach((p, index) => {
    const card = document.createElement("div");
    card.className = "place-card" + (index === 0 ? " active" : "");
    card.dataset.index = index;

    card.innerHTML = `
      <div class="place-name">${p.name}</div>
      <div class="place-meta">
        <span>★ ${p.rating.toFixed(1)}</span>
        <span>${p.type}</span>
      </div>
      <div class="place-tag">${p.tag}</div>
      <div class="place-distance">${p.distance} от вас</div>
    `;

    card.addEventListener("click", () => {
      document.querySelectorAll(".place-card").forEach(c => c.classList.remove("active"));
      card.classList.add("active");
      showPlaceModal(p);
    });

    placesList.appendChild(card);
  });

  function showPlaceModal(place) {
    modalTitle.textContent = place.name;
    modalRating.textContent = `★ ${place.rating.toFixed(1)} · ${place.type}`;
    modalInfo.textContent = `${place.distance} · Просто демонстрация. Здесь потом можно будет открывать маршрут и детали.`;
    modal.style.display = "block";
  }

  modalClose.addEventListener("click", () => {
    modal.style.display = "none";
  });

  // Навигация между экранами
  const navItems = document.querySelectorAll(".nav-item");
  const screens = {
    map: document.getElementById("screen-map"),
    search: document.getElementById("screen-search"),
    account: document.getElementById("screen-account")
  };

  navItems.forEach(item => {
    item.addEventListener("click", () => {
      const target = item.dataset.screen;
      Object.values(screens).forEach(el => el.classList.remove("active"));
      screens[target].classList.add("active");

      navItems.forEach(n => n.classList.remove("active"));
      item.classList.add("active");

      // при переходе на карту закрываем модалку
      if (target === "map") {
        modal.style.display = "none";
      }
    });
  });
</script>
</body>
</html>

