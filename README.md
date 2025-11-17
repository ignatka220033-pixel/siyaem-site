<!DOCTYPE html>
<html lang="ru">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>SIYAEM KOREA — карта мест</title>

  <!-- Иконка -->
  <link rel="icon" type="image/png" href="https://i.imgur.com/9Xn4cYv.png">

  <!-- Kakao Maps SDK -->
  <script src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=5bb3be265d3f64faa0ba9d0b6928b2fb"></script>

  <style>
    body {
      margin: 0;
      font-family: 'Inter', sans-serif;
      background: #030b18;
      color: white;
      overflow: hidden;
    }

    /* ПОДГОНЯЕМ ПОД ВЕСЬ ЭКРАН */
    .page-root {
      min-height: 100vh;
      padding: 10px 20px;
      max-width: none;
      margin: 0;
      box-sizing: border-box;
    }

    /* Хедер */
    .top-bar {
      width: 100%;
      padding: 10px 20px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      box-sizing: border-box;
    }

    .logo {
      font-size: 28px;
      font-weight: 700;
      color: #4da6ff;
    }

    /* Кнопки справа */
    .top-buttons {
      display: flex;
      gap: 10px;
    }

    .btn {
      padding: 8px 18px;
      border-radius: 12px;
      cursor: pointer;
      font-size: 14px;
      font-weight: 600;
      border: none;
      transition: 0.2s;
    }

    .sos-btn {
      background: #ff3b3b;
      color: white;
      box-shadow: 0 0 12px #ff3b3b;
    }

    .acc-btn {
      background: #1d88ff;
      color: white;
    }

    .partner-btn {
      background: #2b2b2b;
      color: #ddd;
    }

    /* Контейнер карты + боковой панели */
    .map-wrapper {
      width: 100%;
      height: calc(100vh - 80px);
      display: flex;
      gap: 20px;
      margin-top: 5px;
    }

    /* Левая панель */
    .sidebar {
      width: 330px;
      height: 100%;
      background: rgba(10, 20, 40, 0.92);
      border-radius: 14px;
      padding: 15px;
      overflow-y: auto;
      backdrop-filter: blur(12px);
    }

    /* СКРЫТЫЙ режим – сворачивается как шторка */
    .sidebar.hidden {
      transform: translateX(-360px);
      opacity: 0;
    }

    .toggle-sidebar-btn {
      position: absolute;
      left: 350px;
      top: 110px;
      z-index: 999;
      background: #1d88ff;
      padding: 8px 12px;
      border-radius: 10px;
      cursor: pointer;
    }

    /* Карта */
    #map {
      flex-grow: 1;
      height: 100%;
      border-radius: 14px;
      overflow: hidden;
    }

    .category-buttons {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
      margin-bottom: 12px;
    }

    .cat-btn {
      padding: 6px 14px;
      border-radius: 10px;
      background: #1b2a44;
      color: #d2e3ff;
      cursor: pointer;
      font-size: 13px;
    }

    .cat-btn.active {
      background: #4da6ff;
      color: #000;
    }

    .place-card {
      background: rgba(255, 255, 255, 0.07);
      margin-bottom: 10px;
      padding: 10px;
      border-radius: 12px;
      line-height: 1.4;
    }

    .place-title {
      font-size: 15px;
      font-weight: 600;
    }

    .place-desc {
      font-size: 13px;
      opacity: 0.82;
    }
  </style>
</head>

<body>

  <div class="page-root">

    <!-- Верхняя панель -->
    <div class="top-bar">
      <div class="logo">SIYAEM KOREA</div>

      <div class="top-buttons">
        <button class="btn partner-btn">Партнёрам</button>
        <button class="btn sos-btn">SOS</button>
        <button class="btn acc-btn">Аккаунт</button>
      </div>
    </div>

    <!-- Контейнер -->
    <div class="map-wrapper">

      <!-- Левая панель -->
      <div class="sidebar" id="sidebar">

        <h3>Места рядом</h3>

        <div class="category-buttons" id="categories">
          <div class="cat-btn active" data-cat="all">Все</div>
          <div class="cat-btn" data-cat="еда">Еда</div>
          <div class="cat-btn" data-cat="магазины">Магазины</div>
          <div class="cat-btn" data-cat="авто">Авто</div>
          <div class="cat-btn" data-cat="салоны">Салоны</div>
          <div class="cat-btn" data-cat="развлечения">Развлечения</div>
        </div>

        <div id="places-list"></div>

      </div>

      <!-- Кнопка сворачивания -->
      <div class="toggle-sidebar-btn" id="toggleSidebar">Меню</div>

      <!-- Карта -->
      <div id="map"></div>
    </div>

  </div>

  <script>
    console.log("JS стартует...");
    // ---- ДАННЫЕ МЕСТ (временные, рандомные) ----
    const placesData = [
      // Сеул
      {
        id: 1,
        name: "Кафе «Сибирь»",
        city: "Сеул",
        category: "еда",
        lat: 37.5563,
        lng: 126.9723,
        desc: "Домашний борщ, пельмени, компоты. Русская кухня в центре Сеула."
      },
      {
        id: 2,
        name: "Магазин «Балтика»",
        city: "Сеул",
        category: "магазины",
        lat: 37.5585,
        lng: 126.9751,
        desc: "Русские продукты, сладости, консервы и крупы."
      },
      {
        id: 3,
        name: "Автосервис «Prestige»",
        city: "Асан",
        category: "авто",
        lat: 36.7892,
        lng: 127.0041,
        desc: "Русскоязычный сервис: ТО, диагностика, помощь с покупкой авто."
      },
      {
        id: 4,
        name: "Салон красоты «Moscow Style»",
        city: "Сувон",
        category: "салоны",
        lat: 37.2636,
        lng: 127.0286,
        desc: "Стрижки, окрашивание, уход за волосами и бровями."
      },
      {
        id: 5,
        name: "Кальянная «Doha Lounge»",
        city: "Пусан",
        category: "развлечения",
        lat: 35.1595,
        lng: 129.0606,
        desc: "Настоящие угли, тихая музыка, настолки, трансляции матчей."
      },
      // ещё несколько заглушек по городам
      {
        id: 6,
        name: "Русская пекарня «Пряник»",
        city: "Инчхон",
        category: "еда",
        lat: 37.4563,
        lng: 126.7052,
        desc: "Хлеб, пироги и сладкая выпечка по домашним рецептам."
      },
      {
        id: 7,
        name: "Мини-маркет «Славянский»",
        city: "Чхонан",
        category: "магазины",
        lat: 36.8151,
        lng: 127.1139,
        desc: "Крупы, сгущёнка, семечки, квас и другие знакомые продукты."
      },
      {
        id: 8,
        name: "Детский центр «Матрешка»",
        city: "Сеул",
        category: "развлечения",
        lat: 37.5201,
        lng: 127.029,
        desc: "Развивающие занятия на русском языке для детей."
      },
      {
        id: 9,
        name: "Тату-студия «Old Money Ink»",
        city: "Сеул",
        category: "салоны",
        lat: 37.5422,
        lng: 127.055,
        desc: "Аккуратные работы, скетчи в стиле old money, консультации."
      },
      {
        id: 10,
        name: "Шиномонтаж «Banana Garage»",
        city: "Асан",
        category: "авто",
        lat: 36.773,
        lng: 127.012,
        desc: "Резина, диски, сезонная переобувка, помощь с подбором."
      }
    ];

    let map;
    let markers = [];
    let currentCategory = "all";

    function initMap() {
      const container = document.getElementById("map");

      const options = {
        center: new kakao.maps.LatLng(36.3, 127.9),
        level: 12
      };

      map = new kakao.maps.Map(container, options);
      console.log("Карта создана");

      renderPlacesList();
      renderMarkers();
      initUI();
    }

    // Создание маркеров по фильтру категории
    function renderMarkers() {
      // Удаляем старые маркеры
      markers.forEach(m => m.setMap(null));
      markers = [];

      const filtered = placesData.filter(p => {
        if (currentCategory === "all") return true;
        return p.category === currentCategory;
      });

      filtered.forEach(place => {
        const markerPosition = new kakao.maps.LatLng(place.lat, place.lng);
        const marker = new kakao.maps.Marker({
          position: markerPosition
        });
        marker.setMap(map);

        kakao.maps.event.addListener(marker, "click", function () {
          map.panTo(markerPosition);
          highlightPlaceCard(place.id);
        });

        markers.push(marker);
      });

      console.log("Маркеры обновлены, всего:", markers.length);
    }

    // Рендер списка мест слева
    function renderPlacesList() {
      const listEl = document.getElementById("places-list");

      const filtered = placesData.filter(p => {
        if (currentCategory === "all") return true;
        return p.category === currentCategory;
      });

      if (filtered.length === 0) {
        listEl.innerHTML = "<div style='opacity:0.7;font-size:13px;'>Пока нет мест в этой категории.</div>";
        return;
      }

      listEl.innerHTML = filtered.map(p => `
        <div class="place-card" data-id="${p.id}">
          <div class="place-title">${p.name}</div>
          <div style="font-size:12px;opacity:0.8;margin-top:2px;">
            ${p.city} · ${categoryToText(p.category)}
          </div>
          <div class="place-desc" style="margin-top:6px;">${p.desc}</div>
        </div>
      `).join("");

      // Навешиваем обработчики на карточки
      Array.from(listEl.querySelectorAll(".place-card")).forEach(card => {
        card.addEventListener("click", () => {
          const id = parseInt(card.getAttribute("data-id"));
          const place = placesData.find(p => p.id === id);
          if (!place) return;

          const pos = new kakao.maps.LatLng(place.lat, place.lng);
          map.panTo(pos);
          highlightPlaceCard(place.id);
        });
      });
    }

    function categoryToText(cat) {
      switch (cat) {
        case "еда": return "Еда";
        case "магазины": return "Магазины";
        case "авто": return "Авто";
        case "салоны": return "Салоны и услуги";
        case "развлечения": return "Развлечения";
        default: return "Разное";
      }
    }

    // Подсветка выбранной карточки
    function highlightPlaceCard(id) {
      const listEl = document.getElementById("places-list");
      Array.from(listEl.querySelectorAll(".place-card")).forEach(card => {
        card.style.border = "1px solid transparent";
        card.style.background = "rgba(255,255,255,0.07)";
      });

      const active = listEl.querySelector(`.place-card[data-id="${id}"]`);
      if (active) {
        active.style.border = "1px solid #4da6ff";
        active.style.background = "rgba(77,166,255,0.15)";
      }
    }

    // UI: категории, шторка, кнопки SOS / Партнёрам / Аккаунт
    function initUI() {
      const catButtons = document.querySelectorAll(".cat-btn");
      catButtons.forEach(btn => {
        btn.addEventListener("click", () => {
          catButtons.forEach(b => b.classList.remove("active"));
          btn.classList.add("active");
          currentCategory = btn.getAttribute("data-cat");
          renderPlacesList();
          renderMarkers();
        });
      });

      // Шторка слева
      const sidebar = document.getElementById("sidebar");
      const toggleBtn = document.getElementById("toggleSidebar");

      toggleBtn.addEventListener("click", () => {
        const isHidden = sidebar.classList.toggle("hidden");
        toggleBtn.textContent = isHidden ? "Меню" : "Скрыть";
      });

      // Партнёрам
      document.querySelector(".partner-btn").addEventListener("click", () => {
        alert(
          "Партнёрство SIYAEM KOREA\n\n" +
          "• Размещение на карте русскоязычных мест в Корее\n" +
          "• QR-плакат в заведение\n" +
          "• Бесплатная базовая фотосъёмка\n" +
          "• Продвижение в наших соцсетях\n\n" +
          "Для подключения: напишите в Telegram / WhatsApp: 010-9809-1703."
        );
      });

      // SOS
      document.querySelector(".sos-btn").addEventListener("click", () => {
        alert(
          "Экстренная помощь SIYAEM KOREA\n\n" +
          "Только реальные ЧП:\n" +
          "• Авария с травмами\n" +
          "• Потеря ребёнка\n" +
          "• Потеря документов\n" +
          "• Срочная госпитализация\n" +
          "• Опасная ситуация, пожар, угрозы\n\n" +
          "Экстренные службы Корея:\n" +
          "• Полиция: 112\n" +
          "• Пожарная / Скорая: 119\n\n" +
          "Если вы не говорите по-корейски — звоните владельцу сервиса: 010-9809-1703."
        );
      });

      // Аккаунт (пока заглушка)
      document.querySelector(".acc-btn").addEventListener("click", () => {
        alert(
          "Личный кабинет будет доступен позже.\n\n" +
          "Здесь можно будет:\n" +
          "• Сохранять избранные места\n" +
          "• Оставлять отзывы\n" +
          "• Управлять своей точкой как партнёр.\n"
        );
      });
    }

    // Запуск карты после загрузки страницы и SDK Kakao
    window.onload = function () {
      if (!window.kakao || !kakao.maps) {
        console.error("Kakao SDK не загрузился");
        return;
      }
      initMap();
    };
  </script>

</body>
</html>
