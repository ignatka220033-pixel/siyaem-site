<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>Siyaem Korea Map — Simple</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <!-- ПРОСТЕЙШИЕ СТИЛИ: КАРТА НА ВЕСЬ ЭКРАН -->
  <style>
    html, body {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      overflow: hidden;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    #map {
      width: 100%;
      height: 100%;
    }

    .debug-panel {
      position: fixed;
      left: 10px;
      bottom: 10px;
      background: rgba(0,0,0,0.6);
      color: #f9fafb;
      padding: 6px 10px;
      border-radius: 8px;
      font-size: 12px;
      z-index: 1000;
    }
  </style>

  <!-- Kakao Map SDK c твоим ключом -->
  <script src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=5bb3be265d3f64faa0ba9d0b6928b2fb"></script>
</head>
<body>

  <!-- КОНТЕЙНЕР КАРТЫ -->
  <div id="map"></div>

  <!-- НЕБОЛЬШАЯ ПАНЕЛЬ ДЛЯ ОТЛАДКИ (МОЖНО УДАЛИТЬ ПОТОМ) -->
  <div class="debug-panel" id="debug">
    Загрузка карты...
  </div>

  <script>
    // ====== БАЗОВЫЕ ДАННЫЕ МЕСТ ======
    // Можно редактировать / добавлять свои точки
    const PLACES = [
      {
        id: "1",
        name: "Кафе «Сибирь»",
        category: "food",
        lat: 37.5665,        // Сеул центр
        lng: 126.9780,
        description: "Русская кухня, тестовая точка."
      },
      {
        id: "2",
        name: "Магазин «Балтика»",
        category: "shop",
        lat: 37.5162,
        lng: 127.1002,
        description: "Русский магазин, тестовая точка."
      },
      {
        id: "3",
        name: "Автосервис «Prestige»",
        category: "auto",
        lat: 36.78,          // регион Асан / Чхонан
        lng: 127.02,
        description: "Русскоязычный автосервис, тестовая точка."
      },
      {
        id: "4",
        name: "Салон красоты «Moscow Style»",
        category: "beauty",
        lat: 35.1595,        // Кванчжу
        lng: 126.8526,
        description: "Салон красоты, тестовая точка."
      },
      {
        id: "5",
        name: "Кальянная «Doha Lounge»",
        category: "fun",
        lat: 35.5384,        // Пусан
        lng: 129.3114,
        description: "Кальянная и отдых, тестовая точка."
      }
    ];

    // Сюда будем складывать маркеры
    const markersById = {};

    // Просто пишет текст в нижнюю панель
    function setDebug(text) {
      const el = document.getElementById("debug");
      if (el) el.textContent = text;
    }

    window.onload = function () {
      setDebug("Окно загружено, проверяем Kakao SDK...");

      if (!window.kakao || !kakao.maps) {
        console.error("Kakao SDK не загрузился. Проверь appkey и домен.");
        setDebug("Ошибка: Kakao SDK не загрузился");
        return;
      }

      const container = document.getElementById("map");

      // Центр Кореи
      const options = {
        center: new kakao.maps.LatLng(36.5, 127.8),
        level: 11
      };

      const map = new kakao.maps.Map(container, options);
      setDebug("Карта создана. Добавляем маркеры...");

      // ====== СОЗДАЁМ МАРКЕРЫ ======
      PLACES.forEach(place => {
        const position = new kakao.maps.LatLng(place.lat, place.lng);

        const marker = new kakao.maps.Marker({
          position,
          map
        });

        markersById[place.id] = marker;

        const infoContent = `
          <div style="padding:8px 10px; font-size:13px; max-width:220px;">
            <b>${place.name}</b><br/>
            <span style="color:#6b7280; font-size:12px;">Категория: ${place.category}</span><br/>
            <span style="font-size:12px;">${place.description}</span>
          </div>
        `;

        const infoWindow = new kakao.maps.InfoWindow({
          content: infoContent,
          removable: true
        });

        kakao.maps.event.addListener(marker, "click", function () {
          infoWindow.open(map, marker);
        });
      });

      setDebug("Готово. Маркеров: " + PLACES.length);
      console.log("Карта и маркеры успешно созданы.");
    };
  </script>

</body>
</html>
