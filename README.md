<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>Siyaem Korea — карта русскоязычных мест</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />

  <!-- Kakao Maps SDK -->
  <script src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=f63924b38de08f3162d1ea0a73766b9a&libraries=services,clusterer"></script>

  <!-- Firebase SDK (compat) -->
  <script src="https://www.gstatic.com/firebasejs/10.13.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.13.0/firebase-auth-compat.js"></script>

  <style>
    * {
      box-sizing: border-box;
    }
    html, body {
      margin: 0;
      padding: 0;
      height: 100%;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "SF Pro Text",
        "Segoe UI", sans-serif;
      background: #020617;
      color: #e5ecff;
    }
    body {
      overflow: hidden;
    }

    /* КАРТА НА ВЕСЬ ЭКРАН */
    #map {
      position: fixed;
      inset: 0;
      width: 100%;
      height: 100%;
      z-index: 1;
    }

    /* ВЕРХНЯЯ ПАНЕЛЬ (поверх карты) */
    .top-bar {
      position: fixed;
      top: 10px;
      left: 50%;
      transform: translateX(-50%);
      width: min(1100px, 100% - 20px);
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 10px 14px;
      border-radius: 999px;
      background: rgba(15, 23, 42, 0.92);
      border: 1px solid rgba(148, 163, 184, 0.4);
      backdrop-filter: blur(18px);
      z-index: 10;
      box-shadow: 0 18px 40px rgba(0, 0, 0, 0.6);
    }

    .logo-block {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .logo-mark {
      width: 30px;
      height: 30px;
      border-radius: 999px;
      background: radial-gradient(circle at 30% 20%, #facc15, #f97316 60%, #0f172a 100%);
      box-shadow: 0 0 18px rgba(250, 204, 21, 0.5);
    }

    .logo-text {
      display: flex;
      flex-direction: column;
      line-height: 1.1;
    }

    .logo-text span:first-child {
      font-size: 12px;
      text-transform: uppercase;
      letter-spacing: 0.14em;
      font-weight: 600;
    }
    .logo-text span:last-child {
      font-size: 11px;
      color: #9ca3af;
    }

    .top-bar-right {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .btn {
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.45);
      padding: 7px 12px;
      background: rgba(15, 23, 42, 0.85);
      color: #e5ecff;
      font-size: 12px;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 6px;
    }
    .btn span.icon {
      font-size: 14px;
    }
    .btn:hover {
      border-color: #38bdf8;
      color: #f9fafb;
    }

    .btn-primary {
      border: none;
      background: linear-gradient(135deg, #38bdf8, #0ea5e9);
      color: #0f172a;
      box-shadow: 0 10px 30px rgba(56, 189, 248, 0.45);
      font-weight: 600;
    }

    .btn-sos {
      border: none;
      background: radial-gradient(circle at 30% 20%, #fecaca, #ef4444 60%, #7f1d1d 100%);
      color: #fff1f2;
      font-weight: 700;
      box-shadow: 0 12px 35px rgba(248, 113, 113, 0.6);
    }

    /* ПАНЕЛЬ ПОИСКА + КАТЕГОРИИ + ФИЛЬТРЫ (как у Яндекс) */
    .floating-panel {
      position: fixed;
      top: 64px;
      left: 50%;
      transform: translateX(-50%);
      width: min(1100px, 100% - 20px);
      z-index: 9;
      margin-top: 10px;
    }

    .search-box {
      width: 100%;
      background: rgba(15, 23, 42, 0.96);
      display: flex;
      align-items: center;
      border: 1px solid #273349;
      border-radius: 12px;
      padding: 8px 12px;
      margin-bottom: 10px;
      backdrop-filter: blur(14px);
    }

    .search-icon {
      font-size: 18px;
      margin-right: 10px;
      color: #8aa4d8;
    }

    .search-input {
      flex: 1;
      background: transparent;
      border: none;
      outline: none;
      color: #dbe6ff;
      font-size: 14px;
    }
    .search-input::placeholder {
      color: #7383a8;
    }

    .categories-yn {
      display: flex;
      gap: 14px;
      padding: 8px 2px 6px;
      overflow-x: auto;
      scrollbar-width: none;
    }
    .categories-yn::-webkit-scrollbar {
      display: none;
    }

    .cat {
      display: flex;
      flex-direction: column;
      align-items: center;
      cursor: pointer;
    }

    .cat-icon {
      width: 55px;
      height: 55px;
      border-radius: 50%;
      background: radial-gradient(circle at 30% 20%, #111827, #020617);
      border: 1px solid #38bdf8;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #f9fafb;
      font-size: 23px;
      transition: 0.18s ease;
      box-shadow: 0 0 0 0 rgba(56, 189, 248, 0.0);
    }

    .cat-name {
      color: #c5d7ff;
      font-size: 11px;
      margin-top: 6px;
      text-align: center;
      width: 70px;
    }

    .cat:hover .cat-icon,
    .cat.active .cat-icon {
      border-color: #67e8f9;
      box-shadow: 0 0 12px rgba(56, 189, 248, 0.9);
      transform: translateY(-3px);
    }

    .filter-bar {
      display: flex;
      gap: 10px;
      padding: 6px 2px 0;
      overflow-x: auto;
      scrollbar-width: none;
    }
    .filter-bar::-webkit-scrollbar {
      display: none;
    }

    .filter {
      background: rgba(15, 23, 42, 0.96);
      border-radius: 10px;
      border: 1px solid #2f3b55;
      padding: 6px 11px;
      font-size: 12px;
      color: #c6d4ff;
      white-space: nowrap;
      cursor: pointer;
      transition: 0.18s ease;
    }

    .filter:hover,
    .filter.active {
      background: #263146;
      border-color: #38bdf8;
      color: #e9f4ff;
    }

    /* Модалка авторизации */
    .modal-backdrop {
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.6);
      display: none;
      align-items: center;
      justify-content: center;
      z-index: 20;
    }
    .modal-backdrop.active {
      display: flex;
    }

    .modal {
      background: radial-gradient(circle at top, #020617, #020617 55%, #000 100%);
      padding: 20px 22px;
      border-radius: 18px;
      width: 100%;
      max-width: 360px;
      box-shadow: 0 18px 40px rgba(0, 0, 0, 0.8);
      border: 1px solid rgba(148, 163, 184, 0.4);
    }
    .modal h3 {
      margin: 0 0 10px 0;
      font-size: 18px;
    }
    .modal-label {
      font-size: 12px;
      margin-top: 8px;
      margin-bottom: 4px;
      color: #cbd5f5;
    }
    .modal-input {
      width: 100%;
      padding: 8px 10px;
      border-radius: 10px;
      border: 1px solid #3b3f63;
      background: #020617;
      color: #e5ecff;
      font-size: 13px;
      outline: none;
    }
    .modal-buttons {
      margin-top: 14px;
      display: flex;
      gap: 8px;
    }
    .modal-btn {
      flex: 1;
      padding: 8px 10px;
      border-radius: 10px;
      border: 0;
      cursor: pointer;
      font-size: 13px;
    }
    .modal-btn.primary {
      background: linear-gradient(135deg, #38bdf8, #0ea5e9);
      color: #0f172a;
      font-weight: 600;
    }
    .modal-btn.outline {
      background: #111827;
      color: #e5ecff;
      border: 1px solid #3b3f63;
    }
    .modal-status {
      margin-top: 8px;
      font-size: 12px;
      min-height: 16px;
      color: #9ca3c7;
    }
    .modal-close-link {
      margin-top: 8px;
      font-size: 12px;
      text-align: right;
      color: #9ca3c7;
      cursor: pointer;
    }

    /* Модалка SOS (упрощённая) */
    .sos-modal-text {
      font-size: 13px;
      color: #e5e7eb;
      margin-bottom: 8px;
    }
    .sos-badge {
      display: inline-block;
      margin: 3px 3px 0 0;
      padding: 4px 8px;
      border-radius: 999px;
      border: 1px solid rgba(248, 113, 113, 0.7);
      color: #fecaca;
      font-size: 12px;
    }

    @media (max-width: 720px) {
      .top-bar {
        border-radius: 18px;
      }
      .logo-text span:first-child {
        font-size: 11px;
      }
      .top-bar-right .btn span.text {
        display: none;
      }
      .top-bar-right .btn {
        padding-inline: 10px;
      }
    }
  </style>
</head>
<body>

<!-- КАРТА -->
<div id="map"></div>

<!-- ВЕРХНЯЯ ПАНЕЛЬ -->
<div class="top-bar">
  <div class="logo-block">
    <div class="logo-mark"></div>
    <div class="logo-text">
      <span>SIYAEM KOREA</span>
      <span>Карта русскоязычных мест</span>
    </div>
  </div>

  <div class="top-bar-right">
    <button class="btn btn-sos" id="sosBtn">
      <span class="icon">🚨</span><span class="text">SOS</span>
    </button>
    <button class="btn" id="partnerBtn">
      <span class="icon">🤝</span><span class="text">Партнёрам</span>
    </button>
    <button class="btn btn-primary" id="loginBtn">
      <span class="icon">👤</span><span class="text">Личный кабинет</span>
    </button>
  </div>
</div>

<!-- ПЛАВАЮЩАЯ ПАНЕЛЬ: ПОИСК + КАТЕГОРИИ + ФИЛЬТРЫ -->
<div class="floating-panel">
  <!-- Поиск -->
  <div class="search-box">
    <div class="search-icon">🔍</div>
    <input type="text" class="search-input" placeholder="Поиск мест и адресов..." />
  </div>

  <!-- Категории (как у Яндекс) -->
  <div class="categories-yn">
    <div class="cat" data-category="food">
      <div class="cat-icon">🍽</div>
      <div class="cat-name">Где поесть</div>
    </div>
    <div class="cat" data-category="market">
      <div class="cat-icon">🛒</div>
      <div class="cat-name">Продукты</div>
    </div>
    <div class="cat" data-category="cafe">
      <div class="cat-icon">☕</div>
      <div class="cat-name">Кафе</div>
    </div>
    <div class="cat" data-category="autoservice">
      <div class="cat-icon">🔧</div>
      <div class="cat-name">Автосервис</div>
    </div>
    <div class="cat" data-category="beauty">
      <div class="cat-icon">💈</div>
      <div class="cat-name">Салоны</div>
    </div>
    <div class="cat" data-category="fastfood">
      <div class="cat-icon">🍔</div>
      <div class="cat-name">Фастфуд</div>
    </div>
    <div class="cat" data-category="fun">
      <div class="cat-icon">🎮</div>
      <div class="cat-name">Развлечения</div>
    </div>
  </div>

  <!-- Фильтры -->
  <div class="filter-bar">
    <div class="filter" data-filter="open">Открыто сейчас</div>
    <div class="filter" data-filter="rating">4.5+ рейтинг</div>
    <div class="filter" data-filter="gift">Подарки</div>
    <div class="filter" data-filter="ru">Русскоязычный</div>
    <div class="filter" data-filter="parking">Парковка</div>
  </div>
</div>

<!-- МОДАЛКА ЛОГИНА -->
<div class="modal-backdrop" id="loginModal">
  <div class="modal">
    <h3>Вход / регистрация Siyaem ID</h3>
    <div class="modal-label">Email</div>
    <input id="authEmail" class="modal-input" type="email" placeholder="you@example.com" />
    <div class="modal-label">Пароль</div>
    <input id="authPassword" class="modal-input" type="password" placeholder="минимум 6 символов" />
    <div class="modal-buttons">
      <button class="modal-btn outline" id="registerBtn">Регистрация</button>
      <button class="modal-btn primary" id="signInBtn">Войти</button>
    </div>
    <div id="authStatus" class="modal-status"></div>
    <div class="modal-close-link" id="closeLoginModal">Закрыть</div>
  </div>
</div>

<!-- МОДАЛКА SOS -->
<div class="modal-backdrop" id="sosModal">
  <div class="modal">
    <h3>Экстренная помощь Siyaem Korea</h3>
    <div class="sos-modal-text">
      Используйте SOS только при реальной угрозе жизни или серьёзной проблеме.
    </div>
    <div class="sos-modal-text">
      <span class="sos-badge">🚓 Полиция — 112</span>
      <span class="sos-badge">🚑 Скорая / пожарные — 119</span>
    </div>
    <div class="sos-modal-text" style="margin-top:10px;">
      В будущем здесь появится список сценариев: авария с травмами, потеря ребёнка, потеря документов, больница и т.д., и твои контакты для помощи.
    </div>
    <div class="modal-close-link" id="closeSosModal">Закрыть</div>
  </div>
</div>

<script>
  // === Firebase init ===
  const firebaseConfig = {
    apiKey: "AIzaSyBdB2mFsj2hVrGCIdy7y4QDK9FcN0_4leA",
    authDomain: "siyaem-639f5.firebaseapp.com",
    projectId: "siyaem-639f5",
    storageBucket: "siyaem-639f5.firebasestorage.app",
    messagingSenderId: "613345654072",
    appId: "1:613345654072:web:db5b0156f0112af879bdcd",
    measurementId: "G-ETHGLXFMNZ"
  };
  firebase.initializeApp(firebaseConfig);
  const auth = firebase.auth();

  // === Kakao Map init ===
  let map;
  function initMap() {
    const container = document.getElementById("map");
    const options = {
      center: new kakao.maps.LatLng(36.5, 127.8), // центр Южной Кореи
      level: 7
    };
    map = new kakao.maps.Map(container, options);

    // Ограничение в рамках Кореи (примерно)
    const sw = new kakao.maps.LatLng(33.0, 124.0);
    const ne = new kakao.maps.LatLng(39.5, 132.5);
    const bounds = new kakao.maps.LatLngBounds(sw, ne);
    kakao.maps.event.addListener(map, "dragend", function () {
      if (!bounds.contain(map.getCenter())) {
        map.setCenter(new kakao.maps.LatLng(36.5, 127.8));
      }
    });

    // TODO: позже добавим маркеры русскоязычных мест
  }

  if (window.kakao && window.kakao.maps) {
    kakao.maps.load(initMap);
  }

  // === Авторизация (логин/регистрация) ===
  const loginBtn = document.getElementById("loginBtn");
  const loginModal = document.getElementById("loginModal");
  const closeLoginModal = document.getElementById("closeLoginModal");
  const registerBtn = document.getElementById("registerBtn");
  const signInBtn = document.getElementById("signInBtn");
  const authStatusEl = document.getElementById("authStatus");

  loginBtn.addEventListener("click", () => {
    authStatusEl.textContent = "";
    loginModal.classList.add("active");
  });

  closeLoginModal.addEventListener("click", () => {
    loginModal.classList.remove("active");
  });

  registerBtn.addEventListener("click", async () => {
    const email = document.getElementById("authEmail").value.trim();
    const password = document.getElementById("authPassword").value.trim();
    authStatusEl.textContent = "Регистрируем...";
    try {
      const userCredential = await auth.createUserWithEmailAndPassword(email, password);
      const user = userCredential.user;
      authStatusEl.textContent = "Успешно! Аккаунт создан: " + user.email;
    } catch (error) {
      authStatusEl.textContent = "Ошибка: " + (error.message || error.code);
    }
  });

  signInBtn.addEventListener("click", async () => {
    const email = document.getElementById("authEmail").value.trim();
    const password = document.getElementById("authPassword").value.trim();
    authStatusEl.textContent = "Входим...";
    try {
      const userCredential = await auth.signInWithEmailAndPassword(email, password);
      const user = userCredential.user;
      authStatusEl.textContent = "Вход выполнен: " + user.email;
      setTimeout(() => {
        loginModal.classList.remove("active");
        alert("Добро пожаловать, " + user.email + "!\nПозже здесь будет личный кабинет с избранным и подарками.");
      }, 700);
    } catch (error) {
      authStatusEl.textContent = "Ошибка: " + (error.message || error.code);
    }
  });

  auth.onAuthStateChanged((user) => {
    if (user) {
      loginBtn.textContent = "Личный кабинет (" + (user.email || "профиль") + ")";
    } else {
      loginBtn.textContent = "Личный кабинет";
    }
  });

  // === SOS modal ===
  const sosBtn = document.getElementById("sosBtn");
  const sosModal = document.getElementById("sosModal");
  const closeSosModal = document.getElementById("closeSosModal");

  sosBtn.addEventListener("click", () => {
    sosModal.classList.add("active");
  });
  closeSosModal.addEventListener("click", () => {
    sosModal.classList.remove("active");
  });

  // Категории / фильтры — пока только визуально (логика фильтрации позже)
  document.querySelectorAll(".cat").forEach((c) => {
    c.addEventListener("click", () => {
      document.querySelectorAll(".cat").forEach((x) => x.classList.remove("active"));
      c.classList.add("active");
      // TODO: фильтрация мест по категории
    });
  });

  document.querySelectorAll(".filter").forEach((f) => {
    f.addEventListener("click", () => {
      f.classList.toggle("active");
      // TODO: применять фильтры к списку мест
    });
  });
</script>
</body>
</html>
