<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Siyaem Korea</title>

  <!-- Leaflet карта -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css"/>
  <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>

  <style>
    /* === ОСНОВНОЙ ДИЗАЙН === */
    body {
      margin: 0;
      padding: 0;
      background: #0f1120;
      color: #fff;
      font-family: Arial, sans-serif;
    }

    /* Верхняя панель */
    .header {
      width: 100%;
      padding: 15px 20px;
      background: #14172b;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .header-title {
      font-size: 22px;
      font-weight: 700;
    }

    /* Кнопки в шапке */
    .header-buttons button {
      background: #2c2f4a;
      color: #a4cfff;
      padding: 10px 15px;
      border: 0;
      margin-left: 10px;
      border-radius: 8px;
      cursor: pointer;
    }

    /* Контейнер */
    .main {
      padding: 20px;
    }

    /* Категории */
    .categories {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
    }

    .category {
      background: #1b1f33;
      padding: 10px 18px;
      border-radius: 10px;
      color: #9cd8ff;
      cursor: pointer;
      border: 1px solid #233;
    }

    /* Карта */
    #map {
      width: 100%;
      height: 500px;
      margin-top: 20px;
      border-radius: 15px;
      overflow: hidden;
      border: 2px solid #2d3150;
    }

    /* Кнопки под картой */
    .sos-button, .partner-button {
      width: 100%;
      background: #2e3559;
      color: #d8e9ff;
      padding: 15px;
      border: 0;
      border-radius: 12px;
      margin-top: 15px;
      font-size: 18px;
      cursor: pointer;
    }

    /* === МОДАЛКА ВХОДА === */
    .modal-backdrop {
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.6);
      display: none;
      align-items: center;
      justify-content: center;
      z-index: 999;
    }
    .modal-backdrop.active {
      display: flex;
    }
    .modal {
      background: #14172b;
      padding: 20px 22px;
      border-radius: 14px;
      width: 100%;
      max-width: 360px;
      box-shadow: 0 15px 40px rgba(0,0,0,0.7);
      border: 1px solid #2d3150;
    }
    .modal h3 {
      margin-top: 0;
      margin-bottom: 10px;
    }
    .modal-label {
      font-size: 13px;
      margin-top: 8px;
      margin-bottom: 4px;
      color: #cbd5f5;
    }
    .modal-input {
      width: 100%;
      padding: 8px 10px;
      border-radius: 8px;
      border: 1px solid #3b3f63;
      background: #0f1224;
      color: #e5ecff;
      font-size: 14px;
      box-sizing: border-box;
    }
    .modal-buttons {
      margin-top: 14px;
      display: flex;
      justify-content: space-between;
      gap: 8px;
    }
    .modal-btn {
      flex: 1;
      padding: 8px 10px;
      border-radius: 8px;
      border: 0;
      cursor: pointer;
      font-size: 14px;
    }
    .modal-btn.primary {
      background: #38bdf8;
      color: #0f1120;
    }
    .modal-btn.outline {
      background: #1b1f33;
      color: #e5ecff;
      border: 1px solid #3b3f63;
    }
    .modal-close {
      text-align: right;
      font-size: 12px;
      margin-top: 6px;
      color: #9ca3c7;
      cursor: pointer;
    }
    .modal-status {
      margin-top: 8px;
      font-size: 12px;
      min-height: 16px;
      color: #9ca3c7;
    }
  </style>
</head>
<body>

<!-- ШАПКА -->
<div class="header">
  <div class="header-title">Siyaem Korea</div>

  <div class="header-buttons">
    <button id="loginBtn">Личный кабинет</button>
    <button onclick="toggleTheme()">Светлая / Тёмная</button>
  </div>
</div>

<!-- ОСНОВНОЙ ЭКРАН -->
<div class="main">

  <h2>Один сервис. Большие возможности.</h2>
  <p style="color:#9bb8ff;">
    Показываем только проверенные русскоязычные места Кореи.
    Помогаем партнёрам зарабатывать, а гостям — находить своё.
  </p>

  <!-- Категории -->
  <div class="categories">
    <div class="category">Еда</div>
    <div class="category">Салоны</div>
    <div class="category">Авто</div>
    <div class="category">Образование</div>
    <div class="category">Развлечения</div>
    <div class="category">Показать на карте</div>
    <div class="category">Оставить заявку</div>
  </div>

  <!-- КАРТА -->
  <div id="map"></div>

  <!-- Кнопки под картой -->
  <button class="partner-button" onclick="openPartner()">Стать партнёром</button>
  <button class="sos-button" onclick="openSOS()">SOS — Экстренная помощь</button>

</div>

<!-- МОДАЛКА ВХОДА / РЕГИСТРАЦИИ -->
<div class="modal-backdrop" id="loginModal">
  <div class="modal">
    <h3>Вход / регистрация Siyaem ID</h3>
    <div class="modal-label">Email</div>
    <input id="authEmail" class="modal-input" type="email" placeholder="you@example.com">

    <div class="modal-label">Пароль</div>
    <input id="authPassword" class="modal-input" type="password" placeholder="минимум 6 символов">

    <div class="modal-buttons">
      <button class="modal-btn outline" id="registerBtn">Регистрация</button>
      <button class="modal-btn primary" id="signInBtn">Войти</button>
    </div>

    <div id="authStatus" class="modal-status"></div>

    <div class="modal-close" id="closeLoginModal">Закрыть</div>
  </div>
</div>

<!-- Firebase SDK -->
<script src="https://www.gstatic.com/firebasejs/10.13.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.13.0/firebase-auth-compat.js"></script>

<script>
  // === ТВОЙ FIREBASE CONFIG (как дал) ===
  const firebaseConfig = {
    apiKey: "AIzaSyBdB2mFsj2hVrGCIdy7y4QDK9FcN0_4leA",
    authDomain: "siyaem-639f5.firebaseapp.com",
    projectId: "siyaem-639f5",
    storageBucket: "siyaem-639f5.firebasestorage.app",
    messagingSenderId: "613345654072",
    appId: "1:613345654072:web:db5b0156f0112af879bdcd",
    measurementId: "G-ETHGLXFMNZ"
  };

  // Инициализация Firebase
  firebase.initializeApp(firebaseConfig);
  const auth = firebase.auth();

  // === КАРТА (пока OSM, затем можно заменить на Kakao) ===
  var map = L.map('map').setView([36.5, 127.8], 7);

  L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
    attribution: "&copy; OSM",
  }).addTo(map);

  // === ПРОСТЫЕ ФУНКЦИИ ===
  function toggleTheme() {
    alert("Переключение темы добавим позже. Сейчас главное — авторизация и карта.");
  }

  function openSOS() {
    alert("Экстренные номера в Корее:\n\n🚓 Полиция — 112\n🚑 Скорая / пожарные — 119\n\nВ будущем здесь будет окно SOS с выбором типа проблемы и твоим номером.");
  }

  function openPartner() {
    alert("Здесь будет форма заявки партнёра (кафе, салон, автосервис и т.д.) с отправкой на твою почту. Это следующий шаг после авторизации.");
  }

  // === ЛОГИКА МОДАЛКИ ВХОДА ===
  const loginBtn      = document.getElementById("loginBtn");
  const loginModal    = document.getElementById("loginModal");
  const closeLoginBtn = document.getElementById("closeLoginModal");
  const signInBtn     = document.getElementById("signInBtn");
  const registerBtn   = document.getElementById("registerBtn");
  const authStatusEl  = document.getElementById("authStatus");

  loginBtn.addEventListener("click", () => {
    authStatusEl.textContent = "";
    loginModal.classList.add("active");
  });

  closeLoginBtn.addEventListener("click", () => {
    loginModal.classList.remove("active");
  });

  // Регистрация нового пользователя
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

  // Вход существующего пользователя
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
        alert("Добро пожаловать, " + user.email + "!\nПозже здесь будет настоящий личный кабинет.");
      }, 800);
    } catch (error) {
      authStatusEl.textContent = "Ошибка: " + (error.message || error.code);
    }
  });

  // Реакция на изменение состояния авторизации (можно использовать позже)
  auth.onAuthStateChanged((user) => {
    if (user) {
      loginBtn.textContent = "Личный кабинет (" + (user.email || "профиль") + ")";
    } else {
      loginBtn.textContent = "Личный кабинет";
    }
  });
</script>

</body>
</html>
