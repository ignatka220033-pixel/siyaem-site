<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>SIYAEM KOREA — карта русскоязычных мест</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <!-- Kakao Maps SDK -->
  <script src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=5bb3be265d3f64faa0ba9d0b6928b2fb&autoload=false"></script>
  <style>
    :root {
      --bg-main: #02091a;
      --bg-panel: rgba(5, 18, 46, 0.96);
      --bg-panel-soft: rgba(6, 22, 59, 0.9);
      --accent: #ff7b3a;
      --accent-soft: rgba(255, 123, 58, 0.18);
      --accent-red: #ff4b5c;
      --accent-blue: #33a9ff;
      --accent-green: #39d98a;
      --text-main: #f5f7ff;
      --text-soft: #9ea9d9;
      --border-soft: rgba(255, 255, 255, 0.08);
      --shadow-soft: 0 18px 40px rgba(0, 0, 0, 0.65);
      --radius-lg: 22px;
      --radius-md: 14px;
      --radius-pill: 999px;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "SF Pro Text",
        "Segoe UI", sans-serif;
      background: radial-gradient(circle at top, #041530 0, #010412 40%, #000);
      color: var(--text-main);
      min-height: 100vh;
      overflow: hidden;
    }

    /* Основной контейнер */
    .app-shell {
      position: relative;
      width: 100vw;
      height: 100vh;
      overflow: hidden;
    }

    /* Карта на фоне 95% */
    #map {
      position: absolute;
      inset: 0;
      width: 100%;
      height: 100%;
      z-index: 1;
    }

    /* Сверху прозрачный градиент для шапки */
    .top-gradient {
      position: absolute;
      inset: 0;
      pointer-events: none;
      z-index: 2;
      background: linear-gradient(
        to bottom,
        rgba(1, 3, 10, 0.9) 0,
        rgba(1, 3, 10, 0.75) 80px,
        transparent 220px
      );
    }

    header {
      position: absolute;
      top: 14px;
      left: 50%;
      transform: translateX(-50%);
      width: min(1120px, 100vw - 40px);
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 14px;
      z-index: 3;
    }

    /* Логотип */
    .logo-badge {
      display: inline-flex;
      align-items: center;
      gap: 10px;
      padding: 10px 16px;
      border-radius: var(--radius-pill);
      background: radial-gradient(circle at left, #ffb13a 0, #ff613a 34%, #150d36 100%);
      box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.12), var(--shadow-soft);
      cursor: default;
    }

    .logo-icon {
      width: 30px;
      height: 30px;
      border-radius: 26px;
      background: conic-gradient(from 210deg, #fff, #ffe39f, #ff9c5a, #ff613a, #ffe39f);
      display: flex;
      align-items: center;
      justify-content: center;
      color: #111;
      font-weight: 800;
      font-size: 16px;
    }

    .logo-text-main {
      font-weight: 700;
      letter-spacing: 0.04em;
      font-size: 15px;
      text-transform: uppercase;
    }

    .logo-text-sub {
      font-size: 11px;
      opacity: 0.9;
      color: #ffe7c5;
    }

    /* Правый блок шапки */
    .header-actions {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .sos-btn {
      position: relative;
      padding: 9px 18px 9px 36px;
      border-radius: var(--radius-pill);
      border: none;
      outline: none;
      background: radial-gradient(circle at top, #ff7464 0, #e5213b 40%, #5e1018 100%);
      color: #fff;
      font-size: 13px;
      font-weight: 600;
      letter-spacing: 0.03em;
      text-transform: uppercase;
      cursor: pointer;
      box-shadow: 0 10px 30px rgba(255, 64, 87, 0.65);
      display: inline-flex;
      align-items: center;
      gap: 10px;
    }

    .sos-dot {
      width: 9px;
      height: 9px;
      border-radius: 50%;
      background: #ffe7e9;
      box-shadow: 0 0 14px #ffe7e9;
      position: absolute;
      left: 16px;
    }

    .pill-btn {
      padding: 8px 16px;
      border-radius: var(--radius-pill);
      border: 1px solid rgba(255, 255, 255, 0.18);
      background: radial-gradient(circle at top, rgba(76, 140, 255, 0.32), rgba(5, 16, 46, 0.95));
      color: var(--text-main);
      font-size: 13px;
      cursor: pointer;
      box-shadow: 0 8px 22px rgba(0, 0, 0, 0.7);
      display: inline-flex;
      align-items: center;
      gap: 8px;
    }

    .pill-btn span.dot {
      width: 6px;
      height: 6px;
      border-radius: 50%;
      background: #7cf2b4;
    }

    .pill-btn-outline {
      padding: 8px 16px;
      border-radius: var(--radius-pill);
      border: 1px solid rgba(255, 255, 255, 0.18);
      background: rgba(5, 14, 40, 0.94);
      color: var(--text-main);
      font-size: 13px;
      cursor: pointer;
      box-shadow: 0 6px 16px rgba(0, 0, 0, 0.75);
      display: inline-flex;
      align-items: center;
      gap: 8px;
    }

    /* Кнопки раскрытия панелей */
    .floating-toggle {
      position: absolute;
      top: 140px;
      z-index: 4;
      padding: 8px 14px;
      border-radius: var(--radius-pill);
      border: 1px solid rgba(255, 255, 255, 0.18);
      background: radial-gradient(circle at top, rgba(63, 113, 255, 0.4), rgba(6, 20, 61, 0.95));
      color: var(--text-main);
      font-size: 12px;
      cursor: pointer;
      box-shadow: 0 12px 28px rgba(0, 0, 0, 0.85);
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .floating-toggle.left {
      left: 18px;
    }

    .floating-toggle.right {
      right: 18px;
    }

    .floating-toggle .bubble {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: var(--accent);
      box-shadow: 0 0 16px rgba(255, 140, 80, 0.9);
    }

    /* ЛЕВАЯ панель мест */
    .side-panel {
      position: absolute;
      top: 110px;
      bottom: 20px;
      width: 360px;
      max-width: calc(100vw - 40px);
      border-radius: var(--radius-lg);
      background: var(--bg-panel);
      box-shadow: var(--shadow-soft);
      border: 1px solid var(--border-soft);
      z-index: 3;
      backdrop-filter: blur(26px);
      display: flex;
      flex-direction: column;
      overflow: hidden;
      transition: transform 0.25s ease, opacity 0.18s ease;
    }

    .side-panel.left {
      left: 18px;
      transform-origin: left center;
    }

    .side-panel.hidden {
      transform: translateX(-120%) scale(0.98);
      opacity: 0;
      pointer-events: none;
    }

    .side-panel-header {
      padding: 14px 18px 10px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.04);
    }

    .panel-title {
      font-size: 15px;
      font-weight: 600;
      margin-bottom: 2px;
    }

    .panel-sub {
      font-size: 11px;
      color: var(--text-soft);
    }

    .search-input {
      margin: 10px 18px 12px;
      position: relative;
    }

    .search-input input {
      width: 100%;
      padding: 9px 12px 9px 30px;
      border-radius: 10px;
      border: 1px solid rgba(255, 255, 255, 0.12);
      background: rgba(4, 13, 34, 0.9);
      color: var(--text-main);
      font-size: 13px;
      outline: none;
    }

    .search-input span.icon {
      position: absolute;
      left: 10px;
      top: 50%;
      transform: translateY(-50%);
      font-size: 13px;
      opacity: 0.6;
    }

    .category-row {
      display: flex;
      flex-wrap: wrap;
      padding: 0 12px 6px;
      gap: 6px;
    }

    .chip {
      padding: 5px 10px;
      border-radius: 999px;
      border: 1px solid rgba(255, 255, 255, 0.16);
      background: rgba(4, 17, 46, 0.9);
      color: var(--text-soft);
      font-size: 11px;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 6px;
    }

    .chip span.dot {
      width: 6px;
      height: 6px;
      border-radius: 50%;
      background: rgba(255, 255, 255, 0.4);
    }

    .chip.active {
      color: #fff;
      border-color: var(--accent);
      background: var(--accent-soft);
    }

    .places-list {
      flex: 1;
      padding: 6px 10px 10px;
      overflow-y: auto;
      scrollbar-width: thin;
      scrollbar-color: #48598b transparent;
    }

    .place-card {
      margin: 6px 4px;
      padding: 9px 10px;
      border-radius: 14px;
      background: var(--bg-panel-soft);
      border: 1px solid rgba(255, 255, 255, 0.06);
      cursor: pointer;
      transition: background 0.16s ease, border-color 0.16s ease, transform 0.1s ease;
    }

    .place-card:hover {
      background: rgba(12, 34, 86, 0.96);
      border-color: rgba(255, 255, 255, 0.16);
      transform: translateY(-1px);
    }

    .place-name {
      font-size: 14px;
      font-weight: 600;
      margin-bottom: 2px;
    }

    .place-meta {
      font-size: 11px;
      color: var(--text-soft);
      margin-bottom: 4px;
    }

    .place-desc {
      font-size: 11px;
      color: #d4ddff;
      line-height: 1.4;
    }

    .place-tags {
      margin-top: 6px;
      display: flex;
      flex-wrap: wrap;
      gap: 4px;
    }

    .tag {
      font-size: 10px;
      padding: 3px 7px;
      border-radius: 999px;
      background: rgba(36, 105, 255, 0.3);
      border: 1px solid rgba(113, 164, 255, 0.5);
      color: #f0f3ff;
    }

    .side-panel-footer {
      padding: 8px 12px 10px;
      border-top: 1px solid rgba(255, 255, 255, 0.06);
      font-size: 10px;
      color: var(--text-soft);
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 6px;
    }

    .lang-switch {
      display: inline-flex;
      background: rgba(5, 14, 36, 0.95);
      border-radius: 999px;
      border: 1px solid rgba(255, 255, 255, 0.14);
      overflow: hidden;
    }

    .lang-switch button {
      border: none;
      background: transparent;
      color: var(--text-soft);
      font-size: 10px;
      padding: 4px 9px;
      cursor: pointer;
      min-width: 30px;
    }

    .lang-switch button.active {
      background: var(--accent-soft);
      color: #fff;
    }

    /* ПРАВАЯ панель аккаунта / партнёров */
    .side-panel.right {
      right: 18px;
      width: 340px;
      transform-origin: right center;
    }

    .side-panel.right.hidden {
      transform: translateX(120%) scale(0.98);
    }

    .account-block {
      padding: 10px 18px 12px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.04);
      font-size: 12px;
    }

    .account-block strong {
      font-size: 13px;
    }

    .contact-row {
      margin-top: 6px;
      display: flex;
      flex-wrap: wrap;
      gap: 4px 8px;
      font-size: 11px;
      color: var(--text-soft);
    }

    .contact-row a {
      color: #b5c7ff;
      text-decoration: none;
    }

    .tariff-grid {
      padding: 10px 14px 6px;
      display: grid;
      grid-template-columns: 1fr;
      gap: 8px;
      font-size: 11px;
    }

    .tariff-card {
      padding: 8px 9px;
      border-radius: 12px;
      background: rgba(7, 19, 52, 0.96);
      border: 1px solid rgba(255, 255, 255, 0.12);
    }

    .tariff-name {
      font-size: 12px;
      font-weight: 600;
      margin-bottom: 2px;
    }

    .tariff-price {
      font-size: 11px;
      color: #ffd5a0;
      margin-bottom: 4px;
    }

    .tariff-benefits {
      list-style: none;
      padding-left: 0;
    }

    .tariff-benefits li {
      margin-bottom: 3px;
      color: var(--text-soft);
    }

    .tariff-benefits li::before {
      content: "• ";
      color: var(--accent-green);
    }

    .tariff-badge {
      display: inline-block;
      padding: 2px 7px;
      border-radius: 999px;
      font-size: 9px;
      text-transform: uppercase;
      letter-spacing: 0.06em;
      background: rgba(66, 230, 149, 0.18);
      border: 1px solid rgba(66, 230, 149, 0.6);
      color: #b9ffdd;
      margin-left: 4px;
    }

    .cta-btn {
      margin: 4px 14px 10px;
      width: calc(100% - 28px);
      padding: 8px 0;
      border-radius: 999px;
      border: none;
      background: radial-gradient(circle at top, #ffb23a 0, #ff6b3a 40%, #7a2a18 100%);
      color: #1a0903;
      font-weight: 600;
      font-size: 12px;
      cursor: pointer;
      box-shadow: 0 12px 30px rgba(0, 0, 0, 0.85);
    }

    .partners-note {
      padding: 4px 14px 10px;
      font-size: 10px;
      color: var(--text-soft);
    }

    /* Модалка SOS */
    .modal-backdrop {
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.74);
      display: none;
      align-items: center;
      justify-content: center;
      z-index: 10;
    }

    .modal-backdrop.open {
      display: flex;
    }

    .modal-card {
      width: min(420px, 100vw - 40px);
      border-radius: 20px;
      background: radial-gradient(circle at top, #2a1723, #050716);
      border: 1px solid rgba(255, 255, 255, 0.16);
      box-shadow: 0 24px 60px rgba(0, 0, 0, 0.9);
      padding: 16px 18px 14px;
    }

    .modal-title {
      font-size: 15px;
      font-weight: 600;
      margin-bottom: 4px;
    }

    .modal-sub {
      font-size: 11px;
      color: #f8bbc3;
      margin-bottom: 10px;
    }

    .sos-grid {
      display: grid;
      grid-template-columns: repeat(3, minmax(0, 1fr));
      gap: 8px;
      margin-bottom: 10px;
    }

    .sos-tile {
      border-radius: 14px;
      padding: 10px 8px;
      background: rgba(130, 37, 59, 0.52);
      border: 1px solid rgba(255, 135, 164, 0.7);
      color: #ffeef3;
      text-align: center;
      font-size: 12px;
    }

    .sos-tile strong {
      display: block;
      font-size: 13px;
      margin-bottom: 4px;
    }

    .sos-num {
      font-size: 12px;
      letter-spacing: 0.06em;
    }

    .sos-note {
      font-size: 10px;
      color: #f0b1bb;
      margin-bottom: 8px;
    }

    .modal-footer {
      display: flex;
      justify-content: flex-end;
      gap: 8px;
    }

    .modal-footer button {
      padding: 6px 12px;
      border-radius: 999px;
      border: 1px solid rgba(255, 255, 255, 0.3);
      background: transparent;
      color: #fff;
      font-size: 11px;
      cursor: pointer;
    }

    .modal-footer button.primary {
      border-color: transparent;
      background: #ff6b81;
      color: #2a070c;
    }

    /* Адаптив */
    @media (max-width: 960px) {
      header {
        flex-direction: column;
        align-items: flex-start;
        gap: 10px;
      }

      .header-actions {
        align-self: flex-end;
      }

      .side-panel {
        width: min(360px, 94vw);
        top: 96px;
        bottom: 14px;
      }

      .floating-toggle {
        top: 100px;
      }
    }

    @media (max-width: 640px) {
      header {
        left: 12px;
        right: 12px;
        transform: none;
        width: auto;
      }

      .header-actions {
        align-self: stretch;
        justify-content: space-between;
      }
    }
  </style>
</head>
<body>
  <div class="app-shell">
    <div id="map"></div>
    <div class="top-gradient"></div>

    <header>
      <div class="logo-badge">
        <div class="logo-icon">S</div>
        <div>
          <div class="logo-text-main">SIYAEM KOREA</div>
          <div class="logo-text-sub">Карта проверенных русскоязычных мест</div>
        </div>
      </div>

      <div class="header-actions">
        <button class="sos-btn" id="sosTrigger">
          <span class="sos-dot"></span>
          SOS — экстренная помощь
        </button>
        <button class="pill-btn" id="partnerToggle">
          <span class="dot"></span>
          Партнёрам
        </button>
        <button class="pill-btn-outline" id="accountToggle">
          Аккаунт
        </button>
      </div>
    </header>

    <!-- Кнопки открытия панелей -->
    <button class="floating-toggle left" id="placesToggle">
      <span class="bubble"></span>
      Места рядом
    </button>
    <button class="floating-toggle right" id="panelToggleRight">
      <span class="bubble"></span>
      Партнёрство / Аккаунт
    </button>

    <!-- Левая панель мест -->
    <aside class="side-panel left" id="placesPanel">
      <div class="side-panel-header">
        <div class="panel-title">Места поблизости</div>
        <div class="panel-sub" id="placesCounter">Только проверенные русскоязычные заведения</div>
      </div>

      <div class="search-input">
        <span class="icon">🔍</span>
        <input id="searchInput" placeholder="Поиск по названию или городу..." />
      </div>

      <div class="category-row" id="categoryRow">
        <!-- сюда JS вставит категории -->
      </div>

      <div class="places-list" id="placesList">
        <!-- сюда JS вставит карточки -->
      </div>

      <div class="side-panel-footer">
        <div>Фильтр: <span id="activeFilterLabel">Все категории</span></div>
        <div class="lang-switch">
          <button data-lang="ru" class="active">RU</button>
          <button data-lang="en">EN</button>
          <button data-lang="ko">KO</button>
        </div>
      </div>
    </aside>

    <!-- Правая панель аккаунта / тарифов -->
    <aside class="side-panel right hidden" id="rightPanel">
      <div class="account-block">
        <strong>Аккаунт / поддержка</strong>
        <div class="contact-row">
          <span>Em Ignat</span>
          <span>·</span>
          <a href="tel:01098091703">010-9809-1703</a>
          <span>·</span>
          <a href="mailto:ignatka220033@gmail.com">ignatka220033@gmail.com</a>
        </div>
        <div class="contact-row">
          Telegram / WhatsApp: <a href="tel:01098091703">тот же номер</a>
        </div>
      </div>

      <div class="tariff-grid">
        <div class="tariff-card">
          <div class="tariff-name">Старт</div>
          <div class="tariff-price">₩29 000 / месяц</div>
          <ul class="tariff-benefits">
            <li>Размещение на карте в нужной категории</li>
            <li>Базовая карточка + контакты</li>
            <li>Поддержка в Telegram / WhatsApp</li>
          </ul>
        </div>
        <div class="tariff-card">
          <div class="tariff-name">Продвижение <span class="tariff-badge">Популярный</span></div>
          <div class="tariff-price">₩59 000 / месяц</div>
          <ul class="tariff-benefits">
            <li>Всё из тарифа «Старт»</li>
            <li>Выделенная карточка и баннер</li>
            <li>1 промо-пост в месяц в наших соцсетях</li>
            <li>Консультация по улучшению сервиса</li>
          </ul>
        </div>
        <div class="tariff-card">
          <div class="tariff-name">Максимум</div>
          <div class="tariff-price">₩99 000 / месяц</div>
          <ul class="tariff-benefits">
            <li>ТОП-позиции в городе по категории</li>
            <li>Бесплатная фотосъёмка заведения</li>
            <li>QR-плакат SIYAEM для гостей</li>
            <li>Личные рекомендации гостям от нас</li>
          </ul>
        </div>
      </div>

      <button class="cta-btn" id="bePartnerBtn">
        Стать партнёром — оставить заявку
      </button>

      <div class="partners-note">
        Долгосрочные партнёры (3 / 6 / 12 месяцев) получают
        дополнительные скидки, приоритет в выдаче и разбор
        маркетинга. Оплата на Shinhan Bank: <strong>110-567-591-809</strong>.
      </div>
    </aside>

    <!-- Модалка SOS -->
    <div class="modal-backdrop" id="sosModal">
      <div class="modal-card">
        <div class="modal-title">Экстренная помощь</div>
        <div class="modal-sub">
          Только при угрозе жизни, тяжёлой аварии, потере ребёнка или документов.
        </div>

        <div class="sos-grid">
          <div class="sos-tile">
            <strong>Полиция</strong>
            <div class="sos-num">112</div>
          </div>
          <div class="sos-tile">
            <strong>Скорая</strong>
            <div class="sos-num">119</div>
          </div>
          <div class="sos-tile">
            <strong>Пожарные</strong>
            <div class="sos-num">119</div>
          </div>
        </div>

        <div class="sos-note">
          Также можете позвонить нам: <strong>010-9809-1703</strong>.
          Но только по серьёзным случаям, а не бытовым вопросам.
        </div>

        <div class="modal-footer">
          <button id="sosCloseBtn">Закрыть</button>
          <button class="primary" onclick="location.href='tel:112'">Вызвать 112</button>
        </div>
      </div>
    </div>

    <!-- JS КОД БУДЕТ НИЖЕ -->
    <script>
      // -----------------------------
      // ДАННЫЕ О МЕСТАХ (распределены по городам и категориям)
      // -----------------------------
      const categories = [
        { id: "all", name: "Все" },
        { id: "food", name: "Еда" },
        { id: "beauty", name: "Салоны" },
        { id: "auto", name: "Авто" },
        { id: "shop", name: "Магазины" },
        { id: "fun", name: "Развлечения" },
      ];

      // Примерные координаты городов (центр)
      const cityCoords = {
        seoul: { lat: 37.5665, lng: 126.978 },
        busan: { lat: 35.1796, lng: 129.0756 },
        incheon: { lat: 37.4563, lng: 126.7052 },
        daegu: { lat: 35.8714, lng: 128.6014 },
        gwangju: { lat: 35.1595, lng: 126.8526 },
      };

      // Рандомные места (в реале заменим на базу)
      const places = [
        // Seoul
        {
          id: 1,
          name: "Кафе «Сибирь»",
          city: "Сеул",
          cityKey: "seoul",
          category: "food",
          desc: "Домашние пельмени, борщ и компоты. Русская кухня в центре Сеула.",
          tags: ["Пельмени", "Борщ", "Домашняя еда"],
          offset: { lat: 0.02, lng: -0.01 },
        },
        {
          id: 2,
          name: "Магазин «Балтика»",
          city: "Сеул",
          cityKey: "seoul",
          category: "shop",
          desc: "Русские продукты, консервы, сладости, крупы.",
          tags: ["Продукты", "Русский магазин"],
          offset: { lat: 0.015, lng: 0.018 },
        },
        {
          id: 3,
          name: "Автосервис «Prestige»",
          city: "Асан / Чхонан",
          cityKey: "seoul",
          category: "auto",
          desc: "Полный сервис для автомобилей, русскоязычные мастера.",
          tags: ["ТО", "Диагностика", "Тюнинг"],
          offset: { lat: -0.035, lng: 0.03 },
        },
        {
          id: 4,
          name: "Кальянная «Doha Lounge»",
          city: "Пусан",
          cityKey: "busan",
          category: "fun",
          desc: "Настольные игры, трансляции и уютная атмосфера.",
          tags: ["Кальян", "Матчи", "Игры"],
          offset: { lat: 0.018, lng: -0.02 },
        },
        {
          id: 5,
          name: "Салон красоты «Moscow Style»",
          city: "Сувон",
          cityKey: "seoul",
          category: "beauty",
          desc: "Стрижки, окрашивание, уход за волосами и бровями.",
          tags: ["Парикмахер", "Маникюр"],
          offset: { lat: -0.02, lng: -0.015 },
        },
        // Busan
        {
          id: 6,
          name: "Кафе «Борщ & Булочки»",
          city: "Пусан",
          cityKey: "busan",
          category: "food",
          desc: "Завтраки, борщ, выпечка и обеды по-домашнему.",
          tags: ["Завтраки", "Обеды"],
          offset: { lat: -0.01, lng: 0.01 },
        },
        {
          id: 7,
          name: "Магазин «Вкус дома»",
          city: "Пусан",
          cityKey: "busan",
          category: "shop",
          desc: "Селёдка, икры, сладости, напитки из СНГ.",
          tags: ["Икра", "Селёдка"],
          offset: { lat: 0.02, lng: 0.02 },
        },
        // Incheon
        {
          id: 8,
          name: "Салон «Северный свет»",
          city: "Инчхон",
          cityKey: "incheon",
          category: "beauty",
          desc: "Косметолог, уход за лицом, массаж.",
          tags: ["Косметолог", "Массаж"],
          offset: { lat: -0.015, lng: -0.02 },
        },
        {
          id: 9,
          name: "Кафе «Пышки»",
          city: "Инчхон",
          cityKey: "incheon",
          category: "food",
          desc: "Пышки, кофе и тёплая атмосфера для встреч.",
          tags: ["Десерты", "Кофе"],
          offset: { lat: 0.01, lng: 0.015 },
        },
        // Daegu
        {
          id: 10,
          name: "Русский магазин «У дома»",
          city: "Тэгу",
          cityKey: "daegu",
          category: "shop",
          desc: "Базовые продукты, крупы, шоколад, напитки.",
          tags: ["Продукты", "Бытовое"],
          offset: { lat: 0.012, lng: -0.018 },
        },
        {
          id: 11,
          name: "Кальян-бар «Скат»",
          city: "Тэгу",
          cityKey: "daegu",
          category: "fun",
          desc: "Музыка, кальян, трансляции футбола.",
          tags: ["Кальян", "Футбол"],
          offset: { lat: -0.014, lng: 0.012 },
        },
        // Gwangju
        {
          id: 12,
          name: "СТО «Korea-Drive»",
          city: "Кванджу",
          cityKey: "gwangju",
          category: "auto",
          desc: "Диагностика, ремонт ходовой, помощь с покупкой авто.",
          tags: ["Ходовая", "Подбор"],
          offset: { lat: 0.016, lng: -0.016 },
        },
        {
          id: 13,
          name: "Кафе «Утро в Питере»",
          city: "Кванджу",
          cityKey: "gwangju",
          category: "food",
          desc: "Сырники, оладьи, завтраки в русском стиле.",
          tags: ["Сырники", "Завтраки"],
          offset: { lat: -0.012, lng: 0.018 },
        },
      ];

      // -----------------------------
      // ИНИЦИАЛИЗАЦИЯ КАРТЫ
      // -----------------------------
      let map;
      let markers = [];
      let activeCategory = "all";
      let activeLang = "ru";

      function initMap() {
        const container = document.getElementById("map");
        const options = {
          center: new kakao.maps.LatLng(36.5, 127.8), // примерно центр Кореи
          level: 12,
        };
        map = new kakao.maps.Map(container, options);

        console.log("Карта создана, добавляем маркеры...");
        renderMarkers();
        console.log("Маркерев загружено:", markers.length);
      }

      // Привязка места к координатам на основе города + offset
      function getPlaceLatLng(place) {
        const base = cityCoords[place.cityKey] || cityCoords["seoul"];
        return new kakao.maps.LatLng(
          base.lat + (place.offset?.lat || 0),
          base.lng + (place.offset?.lng || 0)
        );
      }

      function clearMarkers() {
        markers.forEach((m) => m.setMap(null));
        markers = [];
      }

      function renderMarkers() {
        clearMarkers();

        const filtered = getFilteredPlaces();
        filtered.forEach((p) => {
          const marker = new kakao.maps.Marker({
            map,
            position: getPlaceLatLng(p),
          });
          marker.__placeId = p.id;
          markers.push(marker);

          kakao.maps.event.addListener(marker, "click", () => {
            focusOnPlace(p.id);
          });
        });

        updatePlacesCounter();
      }

      function getFilteredPlaces() {
        const searchText = document
          .getElementById("searchInput")
          .value.trim()
          .toLowerCase();

        return places.filter((p) => {
          const catOk = activeCategory === "all" || p.category === activeCategory;
          if (!catOk) return false;

          if (!searchText) return true;

          const haystack = (
            p.name +
            " " +
            p.city +
            " " +
            p.desc +
            " " +
            p.tags.join(" ")
          )
            .toLowerCase()
            .normalize("NFD")
            .replace(/\p{Diacritic}/gu, "");

          return haystack.includes(searchText);
        });
      }

      function focusOnPlace(id) {
        const place = places.find((p) => p.id === id);
        if (!place) return;

        const pos = getPlaceLatLng(place);
        map.setLevel(6, { animate: true });
        map.panTo(pos);

        // лёгкий всплеск: можно добавить инфо-окно
      }

      // -----------------------------
      // UI: категории, список мест
      // -----------------------------
      function renderCategories() {
        const row = document.getElementById("categoryRow");
        row.innerHTML = "";

        categories.forEach((cat) => {
          const chip = document.createElement("button");
          chip.className = "chip" + (cat.id === "all" ? " active" : "");
          chip.dataset.cat = cat.id;
          chip.innerHTML = `<span class="dot"></span>${cat.name}`;
          chip.addEventListener("click", () => {
            document
              .querySelectorAll(".chip")
              .forEach((el) => el.classList.remove("active"));
            chip.classList.add("active");
            activeCategory = cat.id;
            document.getElementById("activeFilterLabel").textContent =
              cat.name === "Все" ? "Все категории" : cat.name;
            renderMarkers();
            renderPlacesList();
          });
          row.appendChild(chip);
        });
      }

      function renderPlacesList() {
        const list = document.getElementById("placesList");
        list.innerHTML = "";

        const arr = getFilteredPlaces();
        arr.forEach((p) => {
          const card = document.createElement("div");
          card.className = "place-card";
          card.dataset.id = p.id;

          const tagsHtml =
            p.tags && p.tags.length
              ? `<div class="place-tags">${p.tags
                  .map((t) => `<span class="tag">${t}</span>`)
                  .join("")}</div>`
              : "";

          card.innerHTML = `
            <div class="place-name">${p.name}</div>
            <div class="place-meta">${p.city} • ${
            {
              food: "Еда",
              beauty: "Салон",
              auto: "Авто",
              shop: "Магазин",
              fun: "Развлечения",
            }[p.category] || "Место"
          }</div>
            <div class="place-desc">${p.desc}</div>
            ${tagsHtml}
          `;

          card.addEventListener("click", () => focusOnPlace(p.id));
          list.appendChild(card);
        });

        updatePlacesCounter();
      }

      function updatePlacesCounter() {
        const total = getFilteredPlaces().length;
        const all = places.length;
        const text =
          total === all
            ? `Всего мест: ${all}`
            : `Показано: ${total} из ${all}`;
        document.getElementById("placesCounter").textContent = text;
      }

      // -----------------------------
      // Панели и кнопки
      // -----------------------------
      function setupPanels() {
        const placesPanel = document.getElementById("placesPanel");
        const rightPanel = document.getElementById("rightPanel");

        const placesToggle = document.getElementById("placesToggle");
        const panelToggleRight = document.getElementById("panelToggleRight");

        placesToggle.addEventListener("click", () => {
          const hidden = placesPanel.classList.toggle("hidden");
          if (!hidden) {
            rightPanel.classList.add("hidden");
          }
        });

        panelToggleRight.addEventListener("click", () => {
          const hidden = rightPanel.classList.toggle("hidden");
          if (!hidden) {
            placesPanel.classList.add("hidden");
          }
        });

        document
          .getElementById("partnerToggle")
          .addEventListener("click", () => {
            rightPanel.classList.remove("hidden");
            placesPanel.classList.add("hidden");
          });

        document
          .getElementById("accountToggle")
          .addEventListener("click", () => {
            rightPanel.classList.remove("hidden");
            placesPanel.classList.add("hidden");
          });

        document
          .getElementById("searchInput")
          .addEventListener("input", () => {
            renderMarkers();
            renderPlacesList();
          });

        document.querySelectorAll(".lang-switch button").forEach((btn) => {
          btn.addEventListener("click", () => {
            document
              .querySelectorAll(".lang-switch button")
              .forEach((b) => b.classList.remove("active"));
            btn.classList.add("active");
            activeLang = btn.dataset.lang;
            // пока без реального перевода, просто переключатель
          });
        });

        document
          .getElementById("bePartnerBtn")
          .addEventListener("click", () => {
            alert(
              "Чтобы стать партнёром, напишите в Telegram / WhatsApp на 010-9809-1703 или на почту ignatka220033@gmail.com. Мы обсудим категорию, город и подготовим QR-плакат."
            );
          });
      }

      // -----------------------------
      // SOS модалка
      // -----------------------------
      function setupSosModal() {
        const modal = document.getElementById("sosModal");
        const openBtn = document.getElementById("sosTrigger");
        const closeBtn = document.getElementById("sosCloseBtn");

        openBtn.addEventListener("click", () => {
          modal.classList.add("open");
        });

        closeBtn.addEventListener("click", () => {
          modal.classList.remove("open");
        });

        modal.addEventListener("click", (e) => {
          if (e.target === modal) {
            modal.classList.remove("open");
          }
        });
      }

      // -----------------------------
      // ЗАПУСК
      // -----------------------------
      window.onload = function () {
        console.log("window loaded");

        renderCategories();
        renderPlacesList();
        setupPanels();
        setupSosModal();

        if (!window.kakao || !kakao.maps) {
          console.error("Kakao SDK не загрузился");
          return;
        }

        kakao.maps.load(function () {
          initMap();
          console.log("Карта и маркеры успешно созданы.");
        });
      };
    </script>
  </div>
</body>
</html>
