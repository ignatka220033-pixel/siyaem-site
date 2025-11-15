<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>SiyaEm Korea Map</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <style>
    * {
      box-sizing: border-box;
    }
    html, body {
      margin: 0;
      padding: 0;
      height: 100%;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: #020617;
      color: #e5e7eb;
      overflow: hidden; /* чтобы на мобиле не дёргало */
    }

    /* ФОН (вместо карты — пока градиент) */
    #map {
      position: fixed;
      inset: 0;
      background:
        radial-gradient(circle at 10% 0%, #1e293b, #020617 55%, #020617 100%);
      background-image:
        radial-gradient(circle at 20% 30%, rgba(99,102,241,0.22), transparent 65%),
        radial-gradient(circle at 80% 70%, rgba(56,189,248,0.18), transparent 65%);
      z-index: 1;
    }

    /* ВЕРХНЯЯ ПАНЕЛЬ, как у Яндекс */
    .topbar {
      position: fixed;
      top: 8px;
      left: 8px;
      right: 8px;
      height: 52px;
      display: flex;
      align-items: center;
      gap: 8px;
      z-index: 3;
    }

    .topbar-left,
    .topbar-right {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .topbar-left {
      flex: 0 0 auto;
    }

    .topbar-center {
      flex: 1 1 auto;
      display: flex;
      align-items: center;
    }

    .topbar-right {
      flex: 0 0 auto;
      justify-content: flex-end;
    }

    /* Кнопки в шапке */
    .icon-btn {
      min-width: 40px;
      height: 40px;
      border-radius: 999px;
      border: 1px solid rgba(148,163,184,0.6);
      background: rgba(15,23,42,0.96);
      display: inline-flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      color: #e5e7eb;
      font-size: 16px;
      box-shadow: 0 4px 14px rgba(15,23,42,0.8);
    }
    .icon-btn:hover {
      border-color: #38bdf8;
      color: #f9fafb;
    }

    .brand-pill {
      height: 40px;
      border-radius: 999px;
      padding: 0 14px 0 6px;
      background: linear-gradient(135deg, #020617, #020617);
      border: 1px solid rgba(148,163,184,0.6);
      display: flex;
      align-items: center;
      gap: 8px;
      box-shadow: 0 8px 26px rgba(0,0,0,0.7);
      cursor: default;
    }

    .brand-logo {
      width: 26px;
      height: 26px;
      border-radius: 999px;
      background: radial-gradient(circle at 30% 20%, #facc15, #f97316 60%, #b91c1c 100%);
      box-shadow: 0 0 16px rgba(250,204,21,0.7);
    }

    .brand-text {
      display: flex;
      flex-direction: column;
      line-height: 1.1;
    }
    .brand-text strong {
      font-size: 12px;
      letter-spacing: 0.12em;
      text-transform: uppercase;
    }
    .brand-text span {
      font-size: 11px;
      color: #9ca3af;
    }

    /* Поисковая строка, как у Яндекс */
    .search-box {
      width: 100%;
      max-width: 640px;
      margin: 0 auto;
      height: 40px;
      border-radius: 999px;
      background: rgba(15,23,42,0.97);
      border: 1px solid rgba(148,163,184,0.55);
      display: flex;
      align-items: center;
      padding: 0 14px;
      box-shadow: 0 6px 20px rgba(0,0,0,0.7);
    }
    .search-icon {
      font-size: 18px;
      margin-right: 8px;
      color: #9ca3af;
    }
    .search-input {
      flex: 1 1 auto;
      border: none;
      outline: none;
      background: transparent;
      font-size: 14px;
      color: #e5e7eb;
    }
    .search-input::placeholder {
      color: #6b7280;
    }

    .pill-btn {
      height: 40px;
      border-radius: 999px;
      border: 1px solid rgba(148,163,184,0.65);
      background: rgba(15,23,42,0.96);
      padding: 0 14px;
      font-size: 13px;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      cursor: pointer;
      color: #e5e7eb;
      box-shadow: 0 6px 18px rgba(0,0,0,0.7);
    }
    .pill-btn span.icon {
      font-size: 15px;
    }
    .pill-btn.primary {
      background: linear-gradient(135deg, #38bdf8, #6366f1);
      border: none;
      color: #020617;
      font-weight: 600;
      box-shadow: 0 10px 30px rgba(59,130,246,0.55);
    }
    .pill-btn.sos {
      background: radial-gradient(circle at 30% 20%, #fecaca, #ef4444 60%, #7f1d1d 100%);
      border: none;
      color: #fdf2f8;
      font-weight: 700;
      box-shadow: 0 10px 30px rgba(248,113,113,0.8);
    }

    .pill-btn:hover {
      border-color: #38bdf8;
      color: #f9fafb;
    }
    .pill-btn.primary:hover {
      filter: brightness(1.05);
    }
    .pill-btn.sos:hover {
      filter: brightness(1.08);
    }

    /* Плавающая панель (категории + текст) */
    .floating-top {
      position: fixed;
      top: 80px;
      left: 12px;               /* как у Яндекс — прижато к левому краю */
      transform: none;
      width: min(780px, 100% - 24px);
      z-index: 2;
      pointer-events: none;
    }

    .floating-inner {
      pointer-events: auto;
      background: rgba(15,23,42,0.96);
      border-radius: 18px;
      border: 1px solid rgba(148,163,184,0.45);
      padding: 10px 14px 10px;
      box-shadow: 0 18px 40px rgba(0,0,0,0.8);
    }

    /* Заголовок SiyaEm по центру */
    .hero-title {
      text-align: center;
      margin-bottom: 4px;
    }
    .hero-main {
      font-size: 22px;
      font-weight: 700;
      color: #38bdf8;
      letter-spacing: 0.08em;
      text-transform: uppercase;
    }
    .hero-sub {
      margin-top: 3px;
      font-size: 13px;
      color: #e5e7eb;
    }
    .hero-sub-small {
      margin-top: 2px;
      font-size: 11px;
      color: #9ca3af;
    }

    .categories-row {
      display: flex;
      gap: 14px;
      padding: 8px 2px 6px;
      overflow-x: auto;
      scrollbar-width: none;
      justify-content: flex-start; /* Влево */
    }
    .categories-row::-webkit-scrollbar {
      display: none;
    }

    .cat {
      display: flex;
      flex-direction: column;
      align-items: center;
      cursor: pointer;
    }
    .cat-icon {
      width: 52px;
      height: 52px;
      border-radius: 50%;
      background: radial-gradient(circle at 30% 20%, #111827, #020617);
      border: 1px solid #4f46e5;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 22px;
      color: #f9fafb;
      transition: 0.2s;
      box-shadow: 0 0 0 0 rgba(56,189,248,0.0);
    }
    .cat-name {
      font-size: 11px;
      color: #cbd5f5;
      margin-top: 5px;
      text-align: center;
      width: 72px;
    }
    .cat:hover .cat-icon,
    .cat.active .cat-icon {
      border-color: #67e8f9;
      box-shadow: 0 0 12px rgba(56,189,248,0.9);
      transform: translateY(-3px);
    }

    .filters-row {
      display: flex;
      gap: 10px;
      padding: 4px 2px 0;
      overflow-x: auto;
      scrollbar-width: none;
    }
    .filters-row::-webkit-scrollbar {
      display: none;
    }
    .filter-chip {
      padding: 4px 10px 5px;
      border-radius: 999px;
      font-size: 12px;
      background: #020617;
      border: 1px solid #374151;
      color: #d1d5db;
      white-space: nowrap;
      cursor: pointer;
      transition: 0.18s;
    }
    .filter-chip:hover,
    .filter-chip.active {
      background: #111827;
      border-color: #38bdf8;
      color: #e5e7eb;
    }

    /* ЛЕВАЯ ШТОРКА */
    .side-panel {
      position: fixed;
      top: 0;
      bottom: 0;
      background: rgba(15,23,42,0.98);
      box-shadow: 0 0 40px rgba(0,0,0,0.9);
      z-index: 4;
      display: flex;
      flex-direction: column;
      transition: transform 0.22s ease-out;
    }
    .side-panel.left {
      left: 0;
      width: 360px;
      transform: translateX(-100%);
      border-right: 1px solid rgba(31,41,55,0.9);
    }
    .side-panel.right {
      right: 0;
      width: 280px;
      transform: translateX(100%);
      border-left: 1px solid rgba(31,41,55,0.9);
    }
    .side-panel.open.left {
      transform: translateX(0%);
    }
    .side-panel.open.right {
      transform: translateX(0%);
    }

    .panel-header {
      padding: 10px 14px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      border-bottom: 1px solid rgba(31,41,55,0.9);
    }
    .panel-header-title {
      font-size: 14px;
      font-weight: 600;
    }
    .panel-header-sub {
      font-size: 11px;
      color: #9ca3af;
    }

    .panel-close {
      cursor: pointer;
      font-size: 18px;
      color: #9ca3af;
    }
    .panel-close:hover {
      color: #e5e7eb;
    }

    .panel-body {
      padding: 10px 12px;
      overflow-y: auto;
      font-size: 13px;
    }

    .panel-section-title {
      font-size: 12px;
      text-transform: uppercase;
      letter-spacing: 0.1em;
      color: #9ca3af;
      margin: 8px 0 4px;
    }

    .place-card {
      margin-top: 6px;
      padding: 8px 9px;
      border-radius: 12px;
      background: #020617;
      border: 1px solid #111827;
      cursor: pointer;
      transition: 0.18s;
    }
    .place-card:hover {
      border-color: #38bdf8;
      background: #020617;
    }
    .place-title {
      font-size: 13px;
      font-weight: 600;
      margin-bottom: 3px;
    }
    .place-sub {
      font-size: 11px;
      color: #9ca3af;
      margin-bottom: 2px;
    }
    .place-tag {
      display: inline-block;
      padding: 1px 6px;
      border-radius: 999px;
      border: 1px solid #4b5563;
      font-size: 10px;
      color: #d1d5db;
      margin-right: 4px;
    }

    /* ПРАВАЯ ШТОРКА — АККАУНТ */
    .profile-row {
      font-size: 13px;
      margin-top: 8px;
    }
    .profile-label {
      font-size: 11px;
      color: #9ca3af;
      margin-bottom: 3px;
    }
    .profile-box {
      padding: 7px 9px;
      background: #020617;
      border-radius: 10px;
      border: 1px solid #111827;
    }

    .profile-actions {
      margin-top: 8px;
      display: flex;
      flex-direction: column;
      gap: 6px;
    }

    .profile-button {
      border-radius: 8px;
      padding: 7px 9px;
      border: 1px solid #1f2937;
      background: #020617;
      color: #e5e7eb;
      font-size: 13px;
      cursor: pointer;
      text-align: left;
    }
    .profile-button.primary {
      background: linear-gradient(135deg, #38bdf8, #6366f1);
      border: none;
      color: #020617;
      font-weight: 600;
      text-align: center;
    }
    .profile-button:hover {
      border-color: #38bdf8;
    }

    /* Полупрозрачная подложка при открытых шторках */
    .backdrop {
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.35);
      z-index: 3;
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.2s ease-out;
    }
    .backdrop.visible {
      opacity: 1;
      pointer-events: auto;
    }

    @media (max-width: 800px) {
      .floating-top {
        top: 74px;
        left: 8px;
        width: calc(100% - 16px);
      }
      .side-panel.left {
        width: 100%;
      }
      .side-panel.right {
        width: 85%;
      }
      .topbar {
        gap: 4px;
      }
      .brand-pill {
        display: none;
      }
      .pill-btn span.text {
        display: none;
      }
    }
  </style>
</head>
<body>

<div id="map"></div>

<!-- ПОДЛОЖКА ДЛЯ ШТОРОК -->
<div class="backdrop" id="backdrop"></div>

<!-- ВЕРХНЯЯ ПАНЕЛЬ -->
<div class="topbar">
  <div class="topbar-left">
    <button class="icon-btn" id="btnMenu" title="Меню">
      ☰
    </button>
    <div class="brand-pill">
      <div class="brand-logo"></div>
      <div class="brand-text">
        <strong>SIYAEM KOREA</strong>
        <span>Карта русскоязычных мест</span>
      </div>
    </div>
  </div>

  <div class="topbar-center">
    <div class="search-box">
      <div class="search-icon">🔍</div>
      <input class="search-input" placeholder="Поиск места, адреса или категории…" />
    </div>
  </div>

  <div class="topbar-right">
    <button class="pill-btn sos" id="btnSos">
      <span class="icon">🚨</span><span class="text">SOS</span>
    </button>
    <button class="pill-btn" id="btnTheme">
      <span class="icon">🌓</span><span class="text">Тема</span>
    </button>
    <button class="pill-btn primary" id="btnAccount">
      <span class="icon">👤</span><span class="text">Аккаунт</span>
    </button>
  </div>
</div>

<!-- Плавающая панель -->
<div class="floating-top">
  <div class="floating-inner">

    <!-- Заголовок SiyaEm -->
    <div class="hero-title">
      <div class="hero-main">SiyaEm</div>
      <div class="hero-sub">Центр помощи для русскоязычных в Южной Корее</div>
      <div class="hero-sub-small">
        SiyaEm — надёжный помощник в поиске проверенных партнёров, сервисов и мест для жизни.
      </div>
    </div>

    <!-- Категории -->
    <div class="categories-row">
      <div class="cat active">
        <div class="cat-icon">🍽</div>
        <div class="cat-name">Где поесть</div>
      </div>
      <div class="cat">
        <div class="cat-icon">🛒</div>
        <div class="cat-name">Продукты</div>
      </div>
      <div class="cat">
        <div class="cat-icon">☕</div>
        <div class="cat-name">Кафе</div>
      </div>
      <div class="cat">
        <div class="cat-icon">🔧</div>
        <div class="cat-name">Автосервис</div>
      </div>
      <div class="cat">
        <div class="cat-icon">💈</div>
        <div class="cat-name">Салоны</div>
      </div>
      <div class="cat">
        <div class="cat-icon">🍔</div>
        <div class="cat-name">Фастфуд</div>
      </div>
      <div class="cat">
        <div class="cat-icon">🎮</div>
        <div class="cat-name">Развлечения</div>
      </div>
    </div>

    <!-- Фильтры -->
    <div class="filters-row">
      <div class="filter-chip active">Открыто сейчас</div>
      <div class="filter-chip">4.5+ рейтинг</div>
      <div class="filter-chip">Подарки / акции</div>
      <div class="filter-chip">Русскоязычный персонал</div>
      <div class="filter-chip">Есть парковка</div>
    </div>
  </div>
</div>

<!-- ЛЕВАЯ ШТОРКА: список мест -->
<div class="side-panel left" id="leftPanel">
  <div class="panel-header">
    <div>
      <div class="panel-header-title">Места рядом</div>
      <div class="panel-header-sub">Только проверенные русскоязычные заведения</div>
    </div>
    <div class="panel-close" data-close="left">&times;</div>
  </div>
  <div class="panel-body">
    <div class="panel-section-title">Еда и напитки</div>

    <div class="place-card">
      <div class="place-title">Mr. Cook — корейская столовая</div>
      <div class="place-sub">Асан, Дунпо | Русскоязычный персонал</div>
      <div class="place-sub">★ 4.8 · Открыто до 22:00</div>
      <div class="place-tag">Где поесть</div>
      <div class="place-tag">Домашняя кухня</div>
    </div>

    <div class="place-card">
      <div class="place-title">Сибирь Market</div>
      <div class="place-sub">Чхонан | Российские продукты</div>
      <div class="place-sub">★ 4.7 · Закрыто, откроется в 10:00</div>
      <div class="place-tag">Продукты</div>
      <div class="place-tag">Русский магазин</div>
    </div>

    <div class="panel-section-title">Авто и сервис</div>

    <div class="place-card">
      <div class="place-title">Prestige Detailing</div>
      <div class="place-sub">Асан | детейлинг, мойка, керамика</div>
      <div class="place-sub">★ 5.0 · Только по записи</div>
      <div class="place-tag">Автосервис</div>
      <div class="place-tag">Русскоязычный</div>
    </div>

    <div class="panel-section-title">Образ жизни</div>

    <div class="place-card">
      <div class="place-title">Tattoo Studio Siyaem</div>
      <div class="place-sub">Сеул | авторские тату</div>
      <div class="place-sub">★ 4.9 · Свободно завтра</div>
      <div class="place-tag">Развлечения</div>
      <div class="place-tag">Тату</div>
    </div>

    <div style="height: 24px;"></div>
  </div>
</div>

<!-- ПРАВАЯ ШТОРКА: аккаунт -->
<div class="side-panel right" id="rightPanel">
  <div class="panel-header">
    <div>
      <div class="panel-header-title">Аккаунт SiyaEm</div>
      <div class="panel-header-sub">Здесь позже будет личный кабинет</div>
    </div>
    <div class="panel-close" data-close="right">&times;</div>
  </div>
  <div class="panel-body">
    <div class="profile-row">
      <div class="profile-label">Статус</div>
      <div class="profile-box">
        Гость · авторизация по email / Kakao будет добавлена позже.
      </div>
    </div>

    <div class="profile-actions">
      <button class="profile-button primary">Войти / Зарегистрироваться</button>
      <button class="profile-button">Стать партнёром (добавить заведение)</button>
      <button class="profile-button">Мои избранные места</button>
      <button class="profile-button">Настройки уведомлений</button>
    </div>

    <div class="panel-section-title" style="margin-top:12px;">О проекте</div>
    <p style="font-size:12px; color:#9ca3af; line-height:1.4;">
      SiyaEm Korea — сервис для русскоязычных в Южной Корее: проверенные кафе, салоны,
      автосервисы, офисы помощи и другие сервисы. Все точки модератор проверяет лично.
    </p>
  </div>
</div>

<script>
  const leftPanel = document.getElementById('leftPanel');
  const rightPanel = document.getElementById('rightPanel');
  const backdrop = document.getElementById('backdrop');

  function openPanel(side) {
    if (side === 'left') leftPanel.classList.add('open');
    if (side === 'right') rightPanel.classList.add('open');
    backdrop.classList.add('visible');
  }

  function closePanels() {
    leftPanel.classList.remove('open');
    rightPanel.classList.remove('open');
    backdrop.classList.remove('visible');
  }

  document.getElementById('btnMenu').addEventListener('click', () => openPanel('left'));
  document.getElementById('btnAccount').addEventListener('click', () => openPanel('right'));

  document.querySelectorAll('.panel-close').forEach(btn => {
    btn.addEventListener('click', closePanels);
  });
  backdrop.addEventListener('click', closePanels);

  // Переключение категорий
  document.querySelectorAll('.cat').forEach(cat => {
    cat.addEventListener('click', () => {
      document.querySelectorAll('.cat').forEach(c => c.classList.remove('active'));
      cat.classList.add('active');
    });
  });

  // Переключение фильтров
  document.querySelectorAll('.filter-chip').forEach(chip => {
    chip.addEventListener('click', () => chip.classList.toggle('active'));
  });

  // SOS пока просто показывает сообщение
  document.getElementById('btnSos').addEventListener('click', () => {
    alert('SOS: сюда позже подключим экстренную помощь — полиция, скорая, переводчик и т.д.');
  });
</script>
</body>
</html>
