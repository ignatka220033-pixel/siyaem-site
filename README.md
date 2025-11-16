<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>Siyaem Korea — карта русскоязычных мест</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <!-- Стили оформления -->
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    html, body {
      width: 100%;
      height: 100%;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: radial-gradient(circle at top left, #0f172a 0, #020617 45%, #000 100%);
      color: #e5e7eb;
      overflow: hidden;
    }

    body {
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 16px;
      gap: 12px;
    }

    /* Шапка */
    .header {
      width: 100%;
      max-width: 1200px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 8px 14px;
      border-radius: 999px;
      background: rgba(15, 23, 42, 0.9);
      border: 1px solid rgba(148, 163, 184, 0.3);
      backdrop-filter: blur(12px);
      box-shadow: 0 22px 45px rgba(15, 23, 42, 0.8);
    }

    .logo-block {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .logo-circle {
      width: 34px;
      height: 34px;
      border-radius: 50%;
      background: radial-gradient(circle at 30% 20%, #f97316 0, #ea580c 35%, #7c2d12 100%);
      box-shadow: 0 0 25px rgba(248, 113, 22, 0.85);
    }

    .logo-text-main {
      font-size: 16px;
      font-weight: 700;
      letter-spacing: 0.05em;
      text-transform: uppercase;
    }

    .logo-text-sub {
      font-size: 11px;
      color: #9ca3af;
    }

    .header-right {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .top-btn {
      border: none;
      outline: none;
      border-radius: 999px;
      font-size: 12px;
      padding: 6px 14px;
      cursor: pointer;
      color: #e5e7eb;
      background: rgba(30, 64, 175, 0.35);
      border: 1px solid rgba(129, 140, 248, 0.6);
      transition: background 0.15s ease, transform 0.08s ease, box-shadow 0.15s ease;
    }

    .top-btn:hover {
      background: rgba(59, 130, 246, 0.9);
      box-shadow: 0 0 20px rgba(59, 130, 246, 0.6);
      transform: translateY(-1px);
    }

    .top-btn--sos {
      background: radial-gradient(circle at 30% 20%, #f97316 0, #ef4444 40%, #7f1d1d 100%);
      border-color: rgba(248, 113, 22, 0.9);
      box-shadow: 0 0 20px rgba(248, 113, 22, 0.9);
    }

    .top-btn--account {
      background: linear-gradient(135deg, #0ea5e9, #2563eb);
      border-color: rgba(56, 189, 248, 0.9);
    }

    /* Поиск (пока декор) */
    .search-row {
      width: 100%;
      max-width: 1200px;
      display: flex;
      justify-content: center;
    }

    .search-input {
      width: 100%;
      max-width: 640px;
      border-radius: 999px;
      padding: 10px 16px;
      border: 1px solid rgba(148, 163, 184, 0.5);
      background: rgba(15, 23, 42, 0.85);
      color: #e5e7eb;
      font-size: 14px;
      outline: none;
      box-shadow: 0 16px 35px rgba(15, 23, 42, 0.9);
    }

    .search-input::placeholder {
      color: #6b7280;
    }

    /* Контейнер карты */
    .map-wrapper {
      width: 100%;
      max-width: 1200px;
      flex: 1;
      min-height: 0;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .map-panel {
      width: 100%;
      height: calc(100vh - 130px);
      max-height: 720px;
      border-radius: 24px;
      background: #020617;
      border: 1px solid rgba(148, 163, 184, 0.35);
      box-shadow:
        0 28px 55px rgba(15, 23, 42, 0.95),
        0 0 0 1px rgba(15, 23, 42, 0.9);
      overflow: hidden;
      position: relative;
    }

    #map {
      width: 100%;
      height: 100%;
    }

    .status-badge {
      position: absolute;
      left: 14px;
      top: 14px;
      padding: 6px 10px;
      font-size: 11px;
      border-radius: 999px;
      background: rgba(15, 23, 42, 0.9);
      color: #9ca3af;
      border: 1px solid rgba(55, 65, 81, 0.8);
      z-index: 5;
    }

    /* Карточки снизу */
    .cards-strip {
      position: absolute;
      left: 14px;
      right: 14px;
      bottom: 14px;
      display: flex;
      gap: 12px;
      padding: 10px 4px 4px 4px;
      overflow-x: auto;
      scrollbar-width: thin;
      z-index: 6;
    }

    .cards-strip::-webkit-scrollbar {
      height: 6px;
    }
    .cards-strip::-webkit-scrollbar-thumb {
      background: rgba(148, 163, 184, 0.6);
      border-radius: 999px;
    }

    .place-card {
      min-width: 230px;
      max-width: 260px;
      background: radial-gradient(circle at top left, rgba(15, 23, 42, 0.95) 0, rgba(15, 23, 42, 0.98) 35%, rgba(15, 23, 42, 1) 100%);
      border-radius: 16px;
      padding: 10px 12px;
      border: 1px solid rgba(148, 163, 184, 0.4);
      box-shadow: 0 10px 25px rgba(15, 23, 42, 0.95);
      cursor: pointer;
      transition: transform 0.1s ease, box-shadow 0.1s ease, border-color 0.1s ease;
      backdrop-filter: blur(10px);
    }

    .place-card:hover {
      transform: translateY(-3px);
      box-shadow: 0 18px 40px rgba(15, 23, 42, 0.95);
      border-color: rgba(96, 165, 250, 0.9);
    }

    .place-card-title {
      font-size: 14px;
      font-weight: 600;
      margin-bottom: 4px;
    }

    .place-card-category {
      font-size: 11px;
      text-transform: uppercase;
      letter-spacing: 0.08em;
      color: #9ca3af;
      margin-bottom: 6px;
    }

    .place-card-desc {
      font-size: 12px;
      color: #e5e7eb;
      margin-bottom: 6px;
    }

    .place-card-foot {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 11px;
      color: #9ca3af;
    }

    .place-card-tag {
      padding: 2px 8px;
      border-radius: 999px;
      border: 1px solid rgba(96, 165, 250, 0.6);
      color: #bfdbfe;
    }

    @media (max-width: 768px) {
      body {
        padding: 10px;
        gap: 8px;
      }

      .header {
        flex-direction: column;
        align-items: flex-start;
        gap: 6px;
        border-radius: 16px;
      }

      .header-right {
        align-self: flex-end;
      }

      .map-panel {
        height: calc(100vh - 170px);
        border-radius: 18px;
      }

      .search-input {
        font-size: 13px;
        padding: 8px 14px;
      }

      .place-card {
        min-width: 210px;
      }
    }
  </style>

  <!-- Kakao Map SDK -->
  <script src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=5bb3be265d3f64faa0ba9d0b6928b2fb"></script>
</head>
<body>

  <!-- Шапка -->
  <header class="header">
    <div class="logo-block">
      <div class="logo-circle"></div>
      <div>
        <div class="logo-text-main">SIYAEM KOREA</div>
        <div class="logo-text-sub">Карта русскоязычных мест в Южной Корее</div>
      </div>
    </div>
    <div class="header-right">
      <button class="top-btn top-btn--sos">SOS — экстренная помощь</button>
      <button class="top-btn">Тема</button>
      <button class="top-btn top-btn--account">Аккаунт</button>
    </div>
  </header>

  <!-- Поиск (пока не работает, только дизайн) -->
  <div class="search-row">
    <input
      class="search-input"
      type="text"
      placeholder="Поиск места, адреса или категории (пока не работает)" />
  </div>

  <!-- Карта -->
  <main class="map-wrapper">
    <section class="map-panel">
      <div id="map"></div>

      <div class="status-badge" id="status-badge">Загрузка карты…</div>

      <!-- Полоса карточек снизу -->
      <div class="cards-strip" id="cards-strip">
        <!-- Карточки вставятся из JS -->
      </div>
    </section>
  </main>

  <script>
    // ===== ДАННЫЕ МЕСТ (пока тестовые) =====
    const PLACES = [
      {
        id: "1",
        name: "Кафе «Сибирь»",
        category: "Еда",
        lat: 37.5665,
        lng: 126.9780,
        description: "Русская кухня, центр Сеула. Борщ, пельмени, компот.",
        city: "Сеул"
      },
      {
        id: "2",
        name: "Магазин «Балтика»",
        category: "Магазин",
        lat: 37.5162,
        lng: 127.1002,
        description: "Русские продукты, консервы, сладости.",
        city: "Сеул"
      },
      {
        id: "3",
        name: "Автосервис «Prestige»",
        category: "Авто",
        lat: 36.78,
        lng: 127.02,
        description: "Русскоязычный автосервис, помощь с ремонтом и запчастями.",
        city: "Асан / Чхонан"
      },
      {
        id: "4",
        name: "Салон красоты «Moscow Style»",
        category: "Салоны",
        lat: 35.1595,
        lng: 126.8526,
        description: "Стрижки, окрашивание, брови. Мастера говорят по-русски.",
        city: "Кванчжу"
      },
      {
        id: "5",
        name: "Кальянная «Doha Lounge»",
        category: "Развлечения",
        lat: 35.5384,
        lng: 129.3114,
        description: "Кальянная, настольные игры, трансляции матчей.",
        city: "Пусан"
      }
    ];

    const markersById = {};
    const infoWindowsById = {};
    let map = null;

    function setStatus(text) {
      const el = document.getElementById("status-badge");
      if (el) el.textContent = text;
    }

    function closeAllInfoWindows() {
      Object.values(infoWindowsById).forEach(iw => iw.close());
    }

    window.onload = function () {
      setStatus("Окно загружено, инициализируем карту…");

      if (!window.kakao || !kakao.maps) {
        console.error("Kakao SDK не загрузился. Проверь appkey и домен.");
        setStatus("Ошибка: Kakao SDK не загрузился");
        return;
      }

      const container = document.getElementById("map");

      const options = {
        center: new kakao.maps.LatLng(36.5, 127.8),
        level: 11
      };

      map = new kakao.maps.Map(container, options);
      setStatus("Карта создана. Добавляем маркеры…");

      // ===== Маркеры + инфоокна =====
      PLACES.forEach(place => {
        const position = new kakao.maps.LatLng(place.lat, place.lng);

        const marker = new kakao.maps.Marker({
          position,
          map
        });
        markersById[place.id] = marker;

        const infoContent = `
          <div style="padding:8px 10px; font-size:13px; max-width:240px;">
            <b>${place.name}</b><br/>
            <span style="color:#6b7280; font-size:12px;">${place.city} • ${place.category}</span><br/>
            <span style="font-size:12px;">${place.description}</span>
          </div>
        `;

        const infoWindow = new kakao.maps.InfoWindow({
          content: infoContent,
          removable: true
        });
        infoWindowsById[place.id] = infoWindow;

        kakao.maps.event.addListener(marker, "click", () => {
          closeAllInfoWindows();
          infoWindow.open(map, marker);
          map.setCenter(position);
          map.setLevel(6);
        });
      });

      // ===== Рисуем карточки снизу =====
      const cardsStrip = document.getElementById("cards-strip");
      cardsStrip.innerHTML = ""; // на всякий

      PLACES.forEach(place => {
        const card = document.createElement("div");
        card.className = "place-card";
        card.dataset.id = place.id;

        card.innerHTML = `
          <div class="place-card-title">${place.name}</div>
          <div class="place-card-category">${place.city} • ${place.category}</div>
          <div class="place-card-desc">${place.description}</div>
          <div class="place-card-foot">
            <span>Нажми, чтобы показать на карте</span>
            <span class="place-card-tag">Проверено</span>
          </div>
        `;

        card.addEventListener("click", () => {
          const id = place.id;
          const marker = markersById[id];
          const position = new kakao.maps.LatLng(place.lat, place.lng);
          if (!marker || !map) return;

          map.setCenter(position);
          map.setLevel(6);
          closeAllInfoWindows();
          const iw = infoWindowsById[id];
          if (iw) iw.open(map, marker);
        });

        cardsStrip.appendChild(card);
      });

      setStatus("Готово. Маркеров и карточек: " + PLACES.length);
      console.log("Карта, маркеры и карточки успешно созданы.");
    };
  </script>

</body>
</html>
