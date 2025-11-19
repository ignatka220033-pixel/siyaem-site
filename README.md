<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>ETHNOGRAM — шаблон</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", system-ui, sans-serif;
      background: #e5e7eb;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      color: #111827;
    }

    .phone {
      width: min(420px, 100vw);
      height: min(900px, 100vh);
      background: #f3f4f6;
      border-radius: 32px;
      box-shadow: 0 20px 60px rgba(15,23,42,0.35);
      overflow: hidden;
      border: 1px solid #d1d5db;
      position: relative;
    }

    .status-bar {
      height: 22px;
      background: #f3f4f6;
      display: flex;
      align-items: center;
      justify-content: flex-end;
      padding: 0 14px;
      font-size: 10px;
      color: #6b7280;
      gap: 8px;
    }

    .app {
      height: calc(100% - 22px);
      display: flex;
      flex-direction: column;
      background: #ffffff;
    }

    .top-bar {
      height: 52px;
      padding: 8px 14px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      background: #ffffff;
      border-bottom: 1px solid #e5e7eb;
      z-index: 5;
    }

    .top-logo {
      font-weight: 700;
      font-size: 17px;
      padding: 4px 10px;
      border-radius: 999px;
      background: #111827;
      color: #f9fafb;
      letter-spacing: 0.06em;
      text-transform: uppercase;
    }

    .top-logo span { color: #60a5fa; }

    .top-icon-btn {
      width: 30px;
      height: 30px;
      border-radius: 999px;
      border: 1px solid #e5e7eb;
      background: #f9fafb;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 16px;
      color: #4b5563;
    }

    .main { flex: 1; position: relative; overflow: hidden; }

    .screen { position: absolute; inset: 0; display: none; }
    .screen.active { display: block; }

    /* === КАРТА (дневная) === */
    .map-screen { background: #dbeafe; color: #111827; }

    .map-background {
      position: absolute;
      inset: 0;
      background: linear-gradient(to bottom,
        #e5f0ff 0%, #d1e4ff 45%, #c4e0ff 60%, #bfdbfe 100%);
    }

    .map-river {
      position: absolute;
      left: 12%; right: 8%; top: 30%; bottom: 20%;
      border-radius: 999px;
      background: radial-gradient(circle at 10% 0, #93c5fd, #60a5fa);
      opacity: 0.5;
      filter: blur(6px);
    }

    .map-korea-outline {
      position: absolute;
      inset: 14% 12% 18% 20%;
      border-radius: 45% 40% 45% 42%;
      border: 2px solid rgba(37,99,235,0.45);
      box-shadow:
        0 0 25px rgba(59,130,246,0.35),
        0 0 80px rgba(129,199,255,0.25);
    }

    .city-label {
      position: absolute;
      font-size: 11px;
      color: #1f2933;
      background: rgba(255,255,255,0.8);
      padding: 2px 6px;
      border-radius: 999px;
      box-shadow: 0 1px 4px rgba(148,163,184,0.5);
    }

    .city-seoul { top: 26%; left: 45%; }
    .city-busan { bottom: 25%; right: 20%; }
    .city-incheon { top: 23%; left: 37%; }
    .city-daegu { top: 48%; right: 25%; }
    .city-gwangju { bottom: 32%; left: 32%; }

    .map-pin {
      position: absolute;
      width: 22px;
      height: 22px;
      border-radius: 999px 999px 999px 0;
      background: #2563eb;
      transform: rotate(-45deg);
      box-shadow: 0 3px 8px rgba(37,99,235,0.7);
    }

    .map-pin::after {
      content: "";
      position: absolute;
      inset: 5px;
      border-radius: 50%;
      background: radial-gradient(circle at 30% 20%, #eff6ff, #93c5fd);
    }

    .pin-1 { top: 30%; left: 47%; }
    .pin-2 { top: 28%; left: 40%; }
    .pin-3 { top: 54%; right: 24%; }
    .pin-4 { bottom: 30%; left: 33%; }

    .map-search-pill {
      position: absolute;
      left: 50%; transform: translateX(-50%);
      top: 12px;
      width: 92%; max-width: 360px;
      background: #ffffff;
      border-radius: 999px;
      box-shadow: 0 7px 18px rgba(148,163,184,0.65);
      border: 1px solid #e5e7eb;
      padding: 9px 12px;
      display: flex;
      align-items: center;
      gap: 8px;
      font-size: 13px;
      color: #6b7280;
      cursor: pointer;
      z-index: 4;
    }

    .map-search-icon {
      width: 18px;
      height: 18px;
      border-radius: 999px;
      border: 2px solid #9ca3af;
      position: relative;
    }

    .map-search-icon::after {
      content: "";
      position: absolute;
      width: 8px; height: 2px;
      background: #9ca3af;
      border-radius: 999px;
      transform: rotate(45deg);
      right: -4px; bottom: -1px;
    }

    .right-controls {
      position: absolute;
      right: 10px; top: 80px;
      display: flex;
      flex-direction: column;
      gap: 10px;
      z-index: 3;
    }

    .ctrl-btn {
      width: 38px; height: 38px;
      border-radius: 999px;
      background: #ffffff;
      border: 1px solid #e5e7eb;
      box-shadow: 0 4px 12px rgba(148,163,184,0.7);
      display: flex;
      align-items: center;
      justify-content: center;
      color: #374151;
      font-size: 17px;
    }

    .ctrl-btn.small { font-size: 14px; }

    .bottom-sheet {
      position: absolute;
      left: 0; right: 0; bottom: 60px;
      padding: 0 8px 10px;
      z-index: 4;
      pointer-events: none;
    }

    .bottom-sheet-inner {
      pointer-events: auto;
      background: #ffffff;
      border-radius: 22px 22px 0 0;
      box-shadow: 0 -4px 18px rgba(148,163,184,0.8);
      border: 1px solid #e5e7eb;
      padding: 8px 12px 12px;
    }

    .sheet-handle {
      width: 42px; height: 4px;
      border-radius: 999px;
      background: #e5e7eb;
      margin: 4px auto 8px;
    }

    .sheet-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 4px;
    }

    .sheet-title {
      font-size: 14px;
      font-weight: 600;
      color: #111827;
    }

    .sheet-sub { font-size: 11px; color: #6b7280; }

    .sheet-location-row {
      margin-top: 4px;
      display: flex;
      justify-content: space-between;
      font-size: 11px;
      color: #6b7280;
    }

    .sheet-grid {
      margin-top: 8px;
      display: grid;
      grid-template-columns: repeat(2, minmax(0,1fr));
      gap: 8px;
      max-height: 190px;
      overflow-y: auto;
    }

    .sheet-card {
      border-radius: 16px;
      background: linear-gradient(to top, #111827, #1f2937);
      color: #f9fafb;
      padding: 6px 7px 8px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      box-shadow: 0 5px 12px rgba(15,23,42,0.7);
      border: 1px solid transparent;
      cursor: pointer;
    }

    .sheet-card.active {
      border-color: #60a5fa;
      box-shadow:
        0 0 0 1px rgba(96,165,250,0.6),
        0 10px 20px rgba(37,99,235,0.65);
    }

    .sheet-rank { font-size: 15px; font-weight: 700; opacity: 0.85; }
    .sheet-name {
      font-size: 11px; margin-top: 2px;
      white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
    }
    .sheet-distance { font-size: 10px; color: #e5e7eb; margin-top: 2px; }
    .sheet-tag {
      margin-top: 4px;
      align-self: flex-start;
      font-size: 9px;
      padding: 3px 7px;
      border-radius: 999px;
      background: rgba(249,250,251,0.1);
      border: 1px solid rgba(249,250,251,0.4);
    }

    /* НИЖНЯЯ НАВИГАЦИЯ */
    .bottom-nav {
      position: absolute; left: 0; right: 0; bottom: 0;
      height: 60px;
      background: #ffffff;
      border-top: 1px solid #e5e7eb;
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
      color: #6b7280;
      cursor: pointer;
    }

    .nav-icon {
      width: 26px; height: 26px;
      border-radius: 999px;
      display: flex;
      align-items: center;
      justify-content: center;
      border: 1px solid transparent;
      font-size: 16px;
    }

    .nav-item.active .nav-icon {
      border-color: #60a5fa;
      background: #eff6ff;
      color: #1d4ed8;
      box-shadow: 0 0 12px rgba(96,165,250,0.7);
    }

    .nav-item.active span {
      color: #111827;
      font-weight: 500;
    }

    /* === ЭКРАН ОБЪЯВЛЕНИЙ === */
    .ads-screen {
      padding: 10px 14px 70px;
      background: #f9fafb;
      overflow-y: auto;
    }

    .ads-title {
      font-size: 20px;
      font-weight: 700;
      margin-bottom: 4px;
      color: #111827;
    }

    .ads-sub {
      font-size: 12px;
      color: #6b7280;
      margin-bottom: 10px;
    }

    .ads-top-panel {
      display: flex;
      gap: 8px;
      margin-bottom: 10px;
    }

    .ads-add-btn {
      flex: 1;
      background: #111827;
      color: #f9fafb;
      border-radius: 999px;
      padding: 8px 12px;
      font-size: 12px;
      border: none;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 6px;
      box-shadow: 0 3px 8px rgba(15,23,42,0.5);
    }

    .ads-limit-info {
      flex: 1;
      font-size: 11px;
      color: #6b7280;
      background: #e5f2ff;
      border-radius: 12px;
      padding: 7px 9px;
      border: 1px solid #bfdbfe;
    }

    .ads-section-title {
      font-size: 13px;
      font-weight: 600;
      margin: 10px 0 6px;
      color: #111827;
    }

    .ads-card {
      background: #ffffff;
      border-radius: 14px;
      border: 1px solid #e5e7eb;
      padding: 8px 9px;
      font-size: 12px;
      color: #374151;
      box-shadow: 0 2px 6px rgba(148,163,184,0.35);
      margin-bottom: 6px;
    }

    .ads-tag {
      font-size: 10px;
      color: #2563eb;
      margin-bottom: 2px;
      text-transform: uppercase;
      letter-spacing: 0.05em;
    }

    .ads-title-text {
      font-weight: 600;
      margin-bottom: 2px;
    }

    .ads-meta {
      font-size: 10px;
      color: #9ca3af;
      margin-top: 3px;
      display: flex;
      justify-content: space-between;
    }

    /* === ПОИСК === */
    .search-screen {
      padding: 10px 12px 70px;
      background: #f9fafb;
    }

    .search-header { margin-bottom: 8px; }
    .search-title {
      font-size: 18px;
      font-weight: 600;
      margin-bottom: 4px;
      color: #111827;
    }
    .search-sub { font-size: 12px; color: #6b7280; }

    .search-bar-full {
      margin-top: 8px;
      display: flex;
      align-items: center;
      gap: 8px;
      background: #ffffff;
      border-radius: 999px;
      border: 1px solid #e5e7eb;
      padding: 8px 12px;
      box-shadow: 0 4px 10px rgba(148,163,184,0.4);
    }

    .search-chip-row {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
      margin-top: 10px;
      margin-bottom: 10px;
    }

    .chip {
      font-size: 11px;
      padding: 6px 10px;
      border-radius: 999px;
      border: 1px solid #d1d5db;
      background: #ffffff;
      color: #4b5563;
    }

    .search-history-title {
      font-size: 12px;
      margin: 6px 0 4px;
      color: #6b7280;
    }

    .history-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 7px 0;
      border-bottom: 1px solid #e5e7eb;
      font-size: 13px;
    }

    .history-item span:last-child {
      font-size: 11px;
      color: #9ca3af;
    }

    /* === АККАУНТ === */
    .account-screen {
      padding: 12px 14px 70px;
      background: #f9fafb;
      overflow-y: auto;
    }

    .account-header {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 10px;
    }

    .avatar {
      width: 42px;
      height: 42px;
      border-radius: 999px;
      border: 2px solid #60a5fa;
      background: radial-gradient(circle at 30% 20%, #bfdbfe, #60a5fa);
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: 700;
      color: #111827;
    }

    .account-name { font-size: 15px; font-weight: 600; color: #111827; }
    .account-sub { font-size: 11px; color: #6b7280; }

    .section-title {
      font-size: 13px;
      font-weight: 600;
      margin: 10px 0 6px;
      color: #111827;
    }

    .card-row { display: flex; flex-wrap: wrap; gap: 8px; }

    .small-card {
      flex: 1 1 calc(50% - 8px);
      min-width: 130px;
      background: #ffffff;
      border-radius: 14px;
      border: 1px solid #e5e7eb;
      padding: 8px 9px;
      font-size: 11px;
      color: #4b5563;
      box-shadow: 0 2px 6px rgba(148,163,184,0.35);
    }

    .badge {
      display: inline-block;
      font-size: 10px;
      padding: 3px 7px;
      border-radius: 999px;
      border: 1px solid #d1d5db;
      color: #6b7280;
      margin-top: 4px;
      background: #f9fafb;
    }

    .lang-select {
      margin-top: 4px;
      width: 100%;
      padding: 6px 8px;
      border-radius: 10px;
      border: 1px solid #d1d5db;
      background: #ffffff;
      color: #111827;
      font-size: 12px;
    }

    .lang-note {
      font-size: 10px;
      color: #6b7280;
      margin-top: 3px;
    }

    .list-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 7px 0;
      border-bottom: 1px solid #e5e7eb;
      font-size: 12px;
      color: #4b5563;
    }

    .list-item span:last-child {
      color: #9ca3af;
      font-size: 11px;
    }

    @media (max-height: 720px) {
      .phone { border-radius: 24px; }
    }
  </style>
</head>
<body>
<div class="phone">
  <div class="status-bar">
    <span>7:05</span><span>LTE</span><span>🔋 80%</span>
  </div>

  <div class="app">
    <div class="top-bar">
      <div class="top-logo"><span>ETHNO</span>GRAM</div>
      <div class="top-icon-btn">☰</div>
    </div>

    <div class="main">
      <!-- КАРТА -->
      <div class="screen map-screen active" id="screen-map">
        <div class="map-background"></div>
        <div class="map-river"></div>
        <div class="map-korea-outline"></div>

        <div class="city-label city-seoul">Seoul</div>
        <div class="city-label city-incheon">Incheon</div>
        <div class="city-label city-daegu">Daegu</div>
        <div class="city-label city-busan">Busan</div>
        <div class="city-label city-gwangju">Gwangju</div>

        <div class="map-pin pin-1"></div>
        <div class="map-pin pin-2"></div>
        <div class="map-pin pin-3"></div>
        <div class="map-pin pin-4"></div>

        <!-- поисковая пилюля -->
        <div class="map-search-pill" id="map-search-pill">
          <div class="map-search-icon"></div>
          <div>Places, cafes, markets…</div>
        </div>

        <div class="right-controls">
          <div class="ctrl-btn small">⟳</div> <!-- обновить карту -->
          <div class="ctrl-btn">◎</div>      <!-- моё местоположение -->
          <div class="ctrl-btn">＋</div>      <!-- масштаб/zoom -->
          <div class="ctrl-btn small">➤</div> <!-- маршрут/навигация -->
        </div>

        <!-- TOP-10 заведений дня -->
        <div class="bottom-sheet">
          <div class="bottom-sheet-inner">
            <div class="sheet-handle"></div>
            <div class="sheet-header">
              <div>
                <div class="sheet-title">TOP-10 places of the day</div>
                <div class="sheet-sub">Daily picks for foreigners in Korea</div>
              </div>
              <div class="sheet-sub">1.2 km · Seoul</div>
            </div>
            <div class="sheet-location-row">
              <span>Paengseong-eup</span>
              <span>Rotates every day</span>
            </div>
            <div class="sheet-grid" id="sheet-grid"></div>
          </div>
        </div>
      </div>

      <!-- ОБЪЯВЛЕНИЯ -->
      <div class="screen ads-screen" id="screen-ads">
        <div class="ads-title">Объявления</div>
        <div class="ads-sub">
          Доска для гостей и бизнеса: работа, жильё, распродажи, услуги.
        </div>

        <div class="ads-top-panel">
          <button class="ads-add-btn">
            <span>＋ Add new ad</span>
          </button>
          <div class="ads-limit-info">
            Free: 1 ad / month.<br>
            ETHNOGRAM PLUS: 5 ads.<br>
            Business: 15 ads.
          </div>
        </div>

        <div class="ads-section-title">Работа и услуги</div>

        <div class="ads-card">
          <div class="ads-tag">JOB</div>
          <div class="ads-title-text">Ищу работу: водитель / помощник</div>
          Опыт в Корее, есть личный автомобиль, готов к переработкам. Рассмотрю предложения в районе Asan / Cheonan.
          <div class="ads-meta">
            <span>Asan · 19 Nov</span>
            <span>Guest user</span>
          </div>
        </div>

        <div class="ads-card">
          <div class="ads-tag">SERVICE</div>
          <div class="ads-title-text">Русскоязычный мастер по авто</div>
          Диагностика, замена масла, тормоза, помощь с покупкой авто. Консультации для иностранцев.
          <div class="ads-meta">
            <span>Seoul · 19 Nov</span>
            <span>Business profile</span>
          </div>
        </div>

        <div class="ads-section-title">Товары и распродажи</div>

        <div class="ads-card">
          <div class="ads-tag">SALE</div>
          <div class="ads-title-text">Сегодня распродажа пончиков</div>
          Только сегодня: 3 пончика по цене 2. Скидка по ETHNOGRAM-купон+ на кассе.
          <div class="ads-meta">
            <span>Incheon · Today</span>
            <span>Cafe partner</span>
          </div>
        </div>

        <div class="ads-card">
          <div class="ads-tag">MARKET</div>
          <div class="ads-title-text">Продукты из СНГ</div>
          Русский хлеб, кефир, конфеты, крупы. Скидка 5% для пользователей ETHNOGRAM.
          <div class="ads-meta">
            <span>Seoul · 18 Nov</span>
            <span>Market partner</span>
          </div>
        </div>

        <div class="ads-section-title">Жильё и соседи</div>

        <div class="ads-card">
          <div class="ads-tag">ROOM</div>
          <div class="ads-title-text">Ищу соседа в комнату</div>
          Комната в Dunpo, недалеко от станции. Нужен аккуратный сосед, лучше с машиной.
          <div class="ads-meta">
            <span>Dunpo · 18 Nov</span>
            <span>Guest user</span>
          </div>
        </div>

        <div class="ads-card">
          <div class="ads-tag">RENT</div>
          <div class="ads-title-text">Сдам маленький офис под услуги</div>
          Подойдёт для мастера, мини-салона, тату или микс-бизнеса. Помогу с договором.
          <div class="ads-meta">
            <span>Cheonan · 17 Nov</span>
            <span>Owner</span>
          </div>
        </div>
      </div>

      <!-- ПОИСК -->
      <div class="screen search-screen" id="screen-search">
        <div class="search-header">
          <div class="search-title">Search</div>
          <div class="search-sub">
            Поиск по карте: кафе, магазины, сервисы, спорт, развлечения.
          </div>
        </div>

        <div class="search-bar-full">
          <div class="map-search-icon"></div>
          <div style="flex:1; font-size:13px; color:#6b7280;">
            Place, Bus, Subway or Address…
          </div>
        </div>

        <div class="search-chip-row">
          <div class="chip">Coffee & dessert</div>
          <div class="chip">Markets with СНГ продуктами</div>
          <div class="chip">Car service</div>
          <div class="chip">Beauty & Hair</div>
          <div class="chip">Gyms & sports</div>
        </div>

        <div class="search-history-title">Recent</div>
        <div class="history-item">
          <span>Dunpo</span><span>11.18</span>
        </div>
        <div class="history-item">
          <span>McDonald's Gimpo</span><span>11.18</span>
        </div>
        <div class="history-item">
          <span>Russian Market Seoul</span><span>11.16</span>
        </div>
      </div>

      <!-- АККАУНТ -->
      <div class="screen account-screen" id="screen-account">
        <div class="account-header">
          <div class="avatar">IG</div>
          <div>
            <div class="account-name">My ETHNOGRAM</div>
            <div class="account-sub">Coupons, favourites, language, rules</div>
          </div>
        </div>

        <div class="section-title">Coupons & bonuses</div>
        <div class="card-row">
          <div class="small-card">
            До <b>–10%</b> в партнёрских кафе и сервисах.<br>
            Показывай ETHNOGRAM-купон при оплате.
            <div class="badge">Active</div>
          </div>
          <div class="small-card">
            Бесплатный бонус раз в месяц<br>
            для подписчиков (кофе/десерт).
            <div class="badge">Planned</div>
          </div>
        </div>

        <div class="section-title">Favourites</div>
        <div class="small-card">
          Saved places: cafes, markets, beauty, auto, sports.<br>
          Только макет — нажатия никуда не ведут.
          <div class="badge">Demo only</div>
        </div>

        <div class="section-title">App language</div>
        <select class="lang-select">
          <option>Русский</option>
          <option>Қазақ тілі (Kazakhstan)</option>
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

        <div class="section-title">Community & rules</div>
        <div class="list-item">
          <span>Community rules</span><span>Respect & safety</span>
        </div>
        <div class="list-item">
          <span>Driving safety tips</span><span>Important</span>
        </div>
        <div class="list-item">
          <span>Version</span><span>ETHNOGRAM · 0.1 demo</span>
        </div>

        <div class="section-title">Settings & help</div>
        <div class="list-item">
          <span>Support 24/7</span><span>Chat / WhatsApp</span>
        </div>
        <div class="list-item">
          <span>About ETHNOGRAM</span><span>Info</span>
        </div>
        <div class="list-item">
          <span>Log out</span><span>Visual only</span>
        </div>
      </div>

      <!-- НИЖНЕЕ МЕНЮ -->
      <div class="bottom-nav">
        <div class="nav-item active" data-screen="map">
          <div class="nav-icon">📍</div>
          <span>Map</span>
        </div>
        <div class="nav-item" data-screen="ads">
          <div class="nav-icon">📢</div>
          <span>Ads</span>
        </div>
        <div class="nav-item" data-screen="search">
          <div class="nav-icon">🔍</div>
          <span>Search</span>
        </div>
        <div class="nav-item" data-screen="account">
          <div class="nav-icon">👤</div>
          <span>Account</span>
        </div>
      </div>
    </div>
  </div>
</div>

<script>
  // TOP-10 places of the day
  const places = [
    { name: "Glamping Bom", tag: "Glamping / View", distance: "4 km away" },
    { name: "Dutum Jumbo Pocha", tag: "Food & drinks", distance: "1.6 km away" },
    { name: "BBQ Pork Place", tag: "K-BBQ", distance: "2.3 km away" },
    { name: "Dunpo Sports Center", tag: "Sports / Gym", distance: "2.5 km away" },
    { name: "Russian Market Seoul", tag: "SNG products", distance: "3.1 km away" },
    { name: "Han River View Cafe", tag: "Coffee & dessert", distance: "1.2 km away" },
    { name: "Car Rent Korea", tag: "Rent a car", distance: "5.4 km away" },
    { name: "Language Hub", tag: "Korean courses", distance: "800 m away" },
    { name: "Beauty Room K-Style", tag: "Hair & beauty", distance: "1.4 km away" },
    { name: "24/7 Help Point", tag: "Help & translation", distance: "650 m away" }
  ];

  const sheetGrid = document.getElementById("sheet-grid");
  places.forEach((p, index) => {
    const card = document.createElement("div");
    card.className = "sheet-card" + (index === 0 ? " active" : "");
    card.dataset.index = index;
    card.innerHTML = `
      <div class="sheet-rank">${index + 1}</div>
      <div class="sheet-name">${p.name}</div>
      <div class="sheet-distance">${p.distance}</div>
      <div class="sheet-tag">${p.tag}</div>
    `;
    card.addEventListener("click", () => {
      document.querySelectorAll(".sheet-card").forEach(c => c.classList.remove("active"));
      card.classList.add("active");
    });
    sheetGrid.appendChild(card);
  });

  // Навигация
  const navItems = document.querySelectorAll(".nav-item");
  const screens = {
    map: document.getElementById("screen-map"),
    ads: document.getElementById("screen-ads"),
    search: document.getElementById("screen-search"),
    account: document.getElementById("screen-account")
  };

  function openScreen(name) {
    Object.values(screens).forEach(el => el.classList.remove("active"));
    screens[name].classList.add("active");
    navItems.forEach(n => n.classList.remove("active"));
    document.querySelector(`.nav-item[data-screen="${name}"]`).classList.add("active");
  }

  navItems.forEach(item => {
    item.addEventListener("click", () => {
      const target = item.dataset.screen;
      openScreen(target);
    });
  });

  // Нажатие на поисковую пилюлю над картой -> экран поиска
  const mapSearchPill = document.getElementById("map-search-pill");
  mapSearchPill.addEventListener("click", () => {
    openScreen("search");
  });
</script>
</body>
</html>
