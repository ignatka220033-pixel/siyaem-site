<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>Siyaem Korea — карта русскоязычных мест</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

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
    }

    body {
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 12px;
      gap: 10px;
    }

    /* ШАПКА */
    .header {
      width: 100%;
      max-width: 1200px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 8px 14px;
      border-radius: 999px;
      background: rgba(15, 23, 42, 0.9);
      border: 1px solid rgba(148, 163, 184, 0.35);
      backdrop-filter: blur(12px);
      box-shadow: 0 22px 45px rgba(15, 23, 42, 0.85);
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
      box-shadow: 0 0 25px rgba(248, 113, 22, 0.9);
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
      border: 1px solid rgba(129, 140, 248, 0.7);
      transition: background 0.15s ease, box-shadow 0.15s ease, transform 0.08s ease;
    }

    .top-btn:hover {
      background: rgba(59, 130, 246, 0.95);
      box-shadow: 0 0 20px rgba(59, 130, 246, 0.7);
      transform: translateY(-1px);
    }

    .top-btn--sos {
      background: radial-gradient(circle at 30% 20%, #f97316 0, #ef4444 40%, #7f1d1d 100%);
      border-color: rgba(248, 113, 22, 0.9);
      box-shadow: 0 0 20px rgba(248, 113, 22, 0.9);
    }

    .top-btn--account {
      background: linear-gradient(135deg, #0ea5e9, #2563eb);
      border-color: rgba(56, 189, 248, 0.95);
    }

    /* ПОИСК */
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
      padding: 9px 16px;
      border: 1px solid rgba(148, 163, 184, 0.6);
      background: rgba(15, 23, 42, 0.9);
      color: #e5e7eb;
      font-size: 14px;
      outline: none;
      box-shadow: 0 16px 35px rgba(15, 23, 42, 0.95);
    }

    .search-input::placeholder {
      color: #6b7280;
    }

    /* ОСНОВНОЙ ЛЕЙАУТ: СЛЕВА СПИСОК, СПРАВА КАРТА */
    .layout {
      width: 100%;
      max-width: 1200px;
      flex: 1;
      min-height: 0;
      display: flex;
      gap: 10px;
      height: calc(100vh - 130px);
    }

    .sidebar {
      width: 320px;
      min-width: 260px;
      max-width: 360px;
      background: rgba(15, 23, 42, 0.96);
      border-radius: 20px;
      border: 1px solid rgba(148, 163, 184, 0.35);
      box-shadow: 0 24px 50px rgba(15, 23, 42, 0.95);
      display: flex;
      flex-direction: column;
      overflow: hidden;
    }

    .sidebar-header {
      padding: 10px 14px 6px 14px;
      border-bottom: 1px solid rgba(31, 41, 55, 0.9);
    }

    .sidebar-title {
      font-size: 14px;
      font-weight: 600;
      margin-bottom: 4px;
    }

    .sidebar-sub {
      font-size: 11px;
      color: #9ca3af;
    }

    .chips-row {
      padding: 8px 10px;
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
      border-bottom: 1px solid rgba(31, 41, 55, 0.9);
    }

    .chip {
      font-size: 11px;
      padding: 4px 10px;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.7);
      color: #e5e7eb;
      background: rgba(15, 23, 42, 0.9);
      cursor: pointer;
      transition: background 0.12s ease, border-color 0.12s ease, color 0.12s ease;
      white-space: nowrap;
    }

    .chip--active {
      background: rgba(59, 130, 246, 0.9);
      border-color: rgba(96, 165, 250, 1);
      color: #e5e7eb;
    }

    .sidebar-list {
      flex: 1;
      overflow-y: auto;
      padding: 8px 6px 8px 10px;
      scrollbar-width: thin;
    }

    .sidebar-list::-webkit-scrollbar {
      width: 6px;
    }
    .sidebar-list::-webkit-scrollbar-thumb {
      background: rgba(148, 163, 184, 0.7);
      border-radius: 999px;
    }

    .place-item {
      padding: 9px 10px;
      border-radius: 14px;
      border: 1px solid rgba(31, 41, 55, 0.9);
      background: radial-gradient(circle at top left, rgba(15, 23, 42, 1) 0, rgba(15, 23, 42, 0.98) 40%, rgba(15, 23, 42, 1) 100%);
      margin-bottom: 6px;
      cursor: pointer;
      transition: border-color 0.12s ease, background 0.12s ease, transform 0.08s ease, box-shadow 0.1s ease;
    }

    .place-item:hover {
      border-color: rgba(96, 165, 250, 0.85);
      transform: translateY(-1px);
      box-shadow: 0 10px 25px rgba(15, 23, 42, 0.9);
    }

    .place-item--active {
      border-color: rgba(59, 130, 246, 1);
      box-shadow: 0 12px 30px rgba(37, 99, 235, 0.6);
      background: radial-gradient(circle at top left, rgba(37, 99, 235, 0.18) 0, rgba(15, 23, 42, 1) 55%);
    }

    .place-name {
      font-size: 13px;
      font-weight: 600;
      margin-bottom: 2px;
    }

    .place-meta {
      font-size: 11px;
      color: #9ca3af;
      margin-bottom: 4px;
    }

    .place-desc {
      font-size: 12px;
      color: #e5e7eb;
    }

    /* ПРАВАЯ ПАНЕЛЬ С КАРТОЙ */
    .map-panel {
      flex: 1;
      background: #020617;
      border-radius: 22px;
      border: 1px solid rgba(148, 163, 184, 0.35);
      box-shadow: 0 26px 50px rgba(15, 23, 42, 0.95);
      position: relative;
      overflow: hidden;
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
      border: 1px solid rgba(55, 65, 81, 0.85);
      z-index: 5;
    }

    @media (max-width: 900px) {
      body {
        padding: 10px;
      }

      .layout {
        flex-direction: column;
        height: auto;
      }

      .sidebar {
        width: 100%;
        max-width: none;
        order: 2;
        height: 260px;
      }

      .map-panel {
        order: 1;
        height: 320px;
      }
    }
  </style>

  <!-- Kakao Map SDK -->
  <script src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=5bb3be265d3f64faa0ba9d0b6928b2fb"></script>
</head>
<body>

  <!-- ШАПКА -->
  <header class="header">
    <div class="logo-block">
      <div class="logo-circle"></div>
      <div>
        <div class="logo-text-main">SIYAEM KOREA</div>
        <div class="logo-text-sub">Карта проверенных русскоязычных мест в Корее</div>
      </div>
    </div>
    <div class="header-right">
      <button class="top-btn top-btn--sos">SOS — экстренная помощь</button>
      <button class="top-btn">Тема</button>
      <button class="top-btn top-btn--account">Аккаунт</button>
    </div>
  </header>

  <!-- ПОИСК (пока просто дизайн) -->
  <div class="search-row">
    <input
      class="search-input"
      type="text"
      placeholder="Поиск места или адреса (поиск пока не подключён)" />
  </div>

  <!-- ОСНОВНАЯ ОБЛАСТЬ: СЛЕВА СПИСОК, СПРАВА КАРТА -->
  <main class="layout">
    <!-- ЛЕВАЯ ПАНЕЛЬ -->
    <aside class="sidebar">
      <div class="sidebar-header">
        <div class="sidebar-title">Места поблизости</div>
        <div class="sidebar-sub">Только проверенные русскоязычные заведения</div>
      </div>

      <!-- ФИЛЬТРЫ-КНОПКИ (пока визуал, без логики) -->
      <div class="chips-row" id="chips-row">
        <button class="chip chip--active" data-cat="all">Все</button>
        <button class="chip" data-cat="Еда">Еда</button>
        <button class="chip" data-cat="Магазин">Продукты</button>
        <button class="chip" data-cat="Авто">Авто</button>
        <button class="chip" data-cat="Салоны">Салоны</button>
        <button class="chip" data-cat="Развлечения">Развлечения</button>
      </div>

      <!-- СПИСОК МЕСТ -->
      <div class="sidebar-list" id="sidebar-list">
        <!-- Наполняем из JS -->
      </div>
    </aside>

    <!-- ПРАВАЯ ПАНЕЛЬ С КАРТОЙ -->
    <section class="map-panel">
      <div id="map"></div>
      <div class="status-badge" id="status-badge">Загрузка карты…</div>
    </section>
  </main>

  <script>
    // ===== ТЕСТОВЫЕ МЕСТА =====
    const PLACES = [
      {
        id: "1",
        name: "Кафе «Сибирь»",
        category: "Еда",
        lat: 37.5665,
        lng: 126.9780,
        description: "Домашний борщ, пельмени, компоты. Русская кухня в центре Сеула.",
        city: "Сеул"
      },
      {
        id: "2",
        name: "Магазин «Балтика»",
        category: "Магазин",
        lat: 37.5162,
        lng: 127.1002,
        description: "Русские продукты, консервы, сладости, крупы.",
        city: "Сеул"
      },
      {
        id: "3",
        name: "Автосервис «Prestige»",
        category: "Авто",
        lat: 36.78,
        lng: 127.02,
        description: "Полный сервис для автомобиля, русскоязычные мастера.",
        city: "Асан / Чхонан"
      },
      {
        id: "4",
        name: "Салон красоты «Moscow Style»",
        category: "Салоны",
        lat: 35.1595,
        lng: 126.8526,
        description: "Стрижки, окрашивание, уход за волосами и бровями.",
        city: "Кванчжу"
      },
      {
        id: "5",
        name: "Кальянная «Doha Lounge»",
        category: "Развлечения",
        lat: 35.5384,
        lng: 129.3114,
        description: "Кальянная, настольные игры, спортивные трансляции.",
        city: "Пусан"
      }
    ];

    let map = null;
    const markersById = {};
    const infoWindowsById = {};
    const listItemsById = {};

    function setStatus(text) {
      const el = document.getElementById("status-badge");
      if (el) el.textContent = text;
    }

    function closeAllInfoWindows() {
      Object.values(infoWindowsById).forEach(iw => iw.close());
    }

    function deactivateAllListItems() {
      Object.values(listItemsById).forEach(li =>
        li.classList.remove("place-item--active")
      );
    }

    // ИНИЦИАЛИЗАЦИЯ ПО ЗАГРУЗКЕ СТРАНИЦЫ
    window.onload = function () {
      setStatus("Окно загружено, запускаем карту…");

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

      const sidebarList = document.getElementById("sidebar-list");
      sidebarList.innerHTML = "";

      // Создаём маркеры + инфоокна + элементы списка
      PLACES.forEach((place, index) => {
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

        // Элемент списка
        const item = document.createElement("div");
        item.className = "place-item";
        item.dataset.id = place.id;
        item.dataset.category = place.category;

        item.innerHTML = `
          <div class="place-name">${place.name}</div>
          <div class="place-meta">${place.city} • ${place.category}</div>
          <div class="place-desc">${place.description}</div>
        `;

        // Клик по элементу списка
        item.addEventListener("click", () => {
          const id = place.id;
          const marker = markersById[id];
          if (!marker || !map) return;

          const pos = new kakao.maps.LatLng(place.lat, place.lng);
          map.setCenter(pos);
          map.setLevel(6);

          closeAllInfoWindows();
          const iw = infoWindowsById[id];
          if (iw) iw.open(map, marker);

          deactivateAllListItems();
          item.classList.add("place-item--active");
        });

        sidebarList.appendChild(item);
        listItemsById[place.id] = item;

        // Клик по маркеру на карте
        kakao.maps.event.addListener(marker, "click", () => {
          const pos = new kakao.maps.LatLng(place.lat, place.lng);
          map.setCenter(pos);
          map.setLevel(6);

          closeAllInfoWindows();
          infoWindow.open(map, marker);

          deactivateAllListItems();
          item.classList.add("place-item--active");

          // плавно прокрутить список к активному
          item.scrollIntoView({ block: "nearest", behavior: "smooth" });
        });

        // Первое место выделим по умолчанию
        if (index === 0) {
          item.classList.add("place-item--active");
        }
      });

      setStatus("Готово. Всего мест: " + PLACES.length);

      // Фильтры (визуально переключают категорию, без скрытия в этой версии)
      const chipsRow = document.getElementById("chips-row");
      chipsRow.addEventListener("click", (e) => {
        const chip = e.target.closest(".chip");
        if (!chip) return;

        chipsRow.querySelectorAll(".chip").forEach(c =>
          c.classList.remove("chip--active")
        );
        chip.classList.add("chip--active");

        // Логика фильтра пока не включена — позже подключим Firebase/реальные данные
      });
    };
  </script>

</body>
</html>
