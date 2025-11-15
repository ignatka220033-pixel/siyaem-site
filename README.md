<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>Siyaem Korea Map</title>
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
      overflow: hidden;
    }

    /* КАРТА (пока фон) */
    #map {
      position: fixed;
      inset: 0;
      background: radial-gradient(circle at 30% 20%, #1f2937, #020617 60%, #000 100%);
      background-image:
        radial-gradient(circle at 40% 30%, rgba(59,130,246,0.25), transparent 60%),
        radial-gradient(circle at 70% 70%, rgba(45,212,191,0.25), transparent 60%);
      z-index: 1;
    }

    /* ВЕРХНЯЯ ПАНЕЛЬ (как у Яндекс) */
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
      /* центр пока пустой — просто растягиваем, чтобы кнопки справа красиво смотрелись */
    }

    .topbar-right {
      flex: 0 0 auto;
      justify-content: flex-end;
    }

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
      background: linear-gradient(135deg, #111827, #020617);
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

    /* Поисковое поле (теперь используем внутри шторки) */
    .search-box {
      width: 100%;
      height: 36px;
      border-radius: 999px;
      background: rgba(15,23,42,0.97);
      border: 1px solid rgba(148,163,184,0.55);
      display: flex;
      align-items: center;
      padding: 0 10px;
    }
    .search-icon {
      font-size: 16px;
      margin-right: 6px;
      color: #9ca3af;
    }
    .search-input {
      flex: 1 1 auto;
      border: none;
      outline: none;
      background: transparent;
      font-size: 13px;
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
      background: linear-gradient(135deg, #0ea5e9, #22c55e);
      border: none;
      color: #020617;
      font-weight: 600;
      box-shadow: 0 10px 30px rgba(34,197,94,0.6);
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

    /* Категории + фильтры (внутри шторки) */
    .categories-row {
      display: flex;
      gap: 14px;
      padding: 6px 2px 6px;
      overflow-x: auto;
      scrollbar-width: none;
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
      border: 1px solid #38bdf8;
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

    /* Шторки */
    .side-panel {
      position: fixed;
      top: 0;
      bottom: 0;
      width: 360px;
      background: rgba(15,23,42,0.98);
      box-shadow: 0 0 40px rgba(0,0,0,0.9);
      z-index: 4;
      display: flex;
      flex-direction: column;
      transition: transform 0.22s ease-out;
    }
    .side-panel.left {
      left: 0;
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
      padding: 10px 12px 14px;
      overflow-y: auto;
      font-size: 13px;
    }

    .panel-section-title {
      font-size: 12px;
      text-transform: uppercase;
      letter-spacing: 0.1em;
      color: #9ca3af;
      margin: 10px 0 4px;
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

    /* Правая шторка / профиль */
    .profile-section {
      margin-top: 18px;
    }
    .profile-section-title {
      font-size: 11px;
      color: #6b7280;
      text-transform: uppercase;
      letter-spacing: 0.12em;
      margin-bottom: 6px;
    }
    .profile-button {
      width: 100%;
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
      background: linear-gradient(135deg, #0ea5e9, #22c55e);
      border: none;
      color: #020617;
      font-weight: 600;
      text-align: center;
    }
    .profile-button:hover {
      border-color: #38bdf8;
    }

    /* Подложка под шторками */
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

  <!-- центр теперь пустой, без поиска -->
  <div class="topbar-center"></div>

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

<!-- ЛЕВАЯ ШТОРКА: ЛОГО + ПОИСК + КАТЕГОРИИ + ФИЛЬТРЫ + МЕСТА -->
<div class="side-panel left" id="leftPanel">
  <div class="panel-header">
    <div style="display:flex; align-items:center; gap:8px;">
      <div class="brand-logo"></div>
      <div>
        <div class="panel-header-title">Siyaem Korea</div>
        <div class="panel-header-sub">Русскоязычные места в Южной Корее</div>
      </div>
    </div>
    <div class="panel-close" data-close="left">&times;</div>
  </div>

  <div class="panel-body">

    <!-- Поиск теперь здесь, внутри шторки -->
    <div style="margin-bottom:10px;">
      <div class="search-box" style="box-shadow:none;">
        <div class="search-icon">🔍</div>
        <input class="search-input" placeholder="Поиск по местам и адресам…" />
      </div>
    </div>

    <div class="panel-section-title">Категории</div>
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

    <div class="panel-section-title">Фильтры</div>
    <div class="filters-row">
      <div class="filter-chip active">Открыто сейчас</div>
      <div class="filter-chip">4.5+ рейтинг</div>
      <div class="filter-chip">Подарки / акции</div>
      <div class="filter-chip">Русскоязычный персонал</div>
      <div class="filter-chip">Есть парковка</div>
    </div>

    <div class="panel-section-title">Места рядом</div>

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

    <div class="place-card">
      <div class="place-title">Prestige Detailing</div>
      <div class="place-sub">Асан | детейлинг, мойка, керамика</div>
      <div class="place-sub">★ 5.0 · Только по записи</div>
      <div class="place-tag">Автосервис</div>
      <div class="place-tag">Русскоязычный</div>
    </div>

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

<!-- ПРАВАЯ ШТОРКА: ЛИЧНЫЙ КАБИНЕТ -->
<div class="side-panel right" id="rightPanel">
  <div class="panel-header">
    <div style="display:flex; align-items:center; gap:10px;">
      <div style="
        width:42px; height:42px;
        border-radius:50%;
        background:#1f2937;
        display:flex; align-items:center; justify-content:center;
        font-size:20px; color:#9ca3af;">
        👤
      </div>
      <div>
        <div class="panel-header-title">Гость</div>
        <div class="panel-header-sub">Войдите, чтобы сохранять места</div>
      </div>
    </div>
    <div class="panel-close" data-close="right">&times;</div>
  </div>

  <div class="panel-body">

    <button class="profile-button primary" style="margin-bottom:12px;">
      Войти / Зарегистрироваться
    </button>

    <div class="profile-section">
      <div class="profile-section-title">Профиль</div>

      <button class="profile-button">Мои данные</button>
      <button class="profile-button">Мои места</button>
      <button class="profile-button">Избранное</button>
      <button class="profile-button">История просмотренных</button>
    </div>

    <div class="profile-section">
      <div class="profile-section-title">Сервис</div>

      <button class="profile-button">Стать партнёром</button>
      <button class="profile-button">Мои заведения</button>
      <button class="profile-button">Поддержка</button>
    </div>

    <div class="profile-section">
      <div class="profile-section-title">О приложении</div>
      
      <button class="profile-button">О Siyaem Korea</button>
      <button class="profile-button">Версия 0.1 (beta)</button>
    </div>

    <div class="profile-section">
      <button class="profile-button" style="border-color:#7f1d1d; color:#fca5a5;">
        Выйти
      </button>
    </div>

    <div style="height:30px;"></div>
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

  // категории внутри шторки
  document.querySelectorAll('.cat').forEach(cat => {
    cat.addEventListener('click', () => {
      document.querySelectorAll('.cat').forEach(c => c.classList.remove('active'));
      cat.classList.add('active');
    });
  });

  // фильтры внутри шторки
  document.querySelectorAll('.filter-chip').forEach(chip => {
    chip.addEventListener('click', () => chip.classList.toggle('active'));
  });

  // SOS пока просто алерт
  document.getElementById('btnSos').addEventListener('click', () => {
    alert('SOS: позже сюда подключим реальную экстренную помощь — полиция, скорая, переводчик и т.д.');
  });
</script>
</body>
</html>
