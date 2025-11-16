<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>Siyaem Korea Map</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <style>
    html, body {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      overflow: hidden;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: #020617;
      color: #e5e7eb;
    }

    #map {
      width: 100%;
      height: 100%;
    }

    /* Небольшой полупрозрачный заголовок (потом доработаем под дизайн) */
    .top-banner {
      position: absolute;
      z-index: 1000;
      top: 12px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(15, 23, 42, 0.9);
      padding: 6px 14px;
      border-radius: 999px;
      font-size: 13px;
      border: 1px solid rgba(148, 163, 184, 0.3);
      backdrop-filter: blur(10px);
    }
  </style>

  <!-- Kakao Maps SDK: используем твой JavaScript Key -->
  <script src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=5bb3be265d3f64faa0ba9d0b6928b2fb"></script>
</head>
<body>

  <!-- Заголовок поверх карты (просто чтобы видеть, что страница живая) -->
  <div class="top-banner">
    Siyaem Korea — тест русскоязычных мест на карте
  </div>

  <!-- Контейнер карты -->
  <div id="map"></div>

  <script>
    // ====== ШАГ 1. ТЕСТОВЫЕ РУССКОЯЗЫЧНЫЕ МЕСТА ======
    // category: food / beauty / auto / fun / service / shop и т.д.
    const PLACES = [
      {
        id: "place_1",
        name: "Кафе «Сибирь»",
        category: "food",
        lat: 37.5665,   // Сеул центр
        lng: 126.9780,
        city: "Сеул",
        address: "Рядом с мэрией (пример)",
        description: "Русская кухня, борщ, пельмени, оливье. Тестовая точка.",
      },
      {
        id: "place_2",
        name: "Русский магазин «Балтика»",
        category: "shop",
        lat: 37.5162,   // Сеул, район Бундан
        lng: 127.1002,
        city: "Сеул",
        address: "Окрестности Ганнама (пример)",
        description: "Продукты из России и СНГ. Тестовая точка.",
      },
      {
        id: "place_3",
        name: "Автосервис «Prestige»",
        category: "auto",
        lat: 36.78,     // Твой регион (Асан / Чхонан, примерно)
        lng: 127.02,
        city: "Асан / Чхонан",
        address: "Примерное место в Чхонане",
        description: "Русскоязычный автосервис. Тестовая точка для авто.",
      },
      {
        id: "place_4",
        name: "Салон красоты «Moscow Style»",
        category: "beauty",
        lat: 35.1595,   // Кванчжу (для разнообразия)
        lng: 126.8526,
        city: "Кванчжу",
        address: "Центр города, пример",
        description: "Услуги для русскоязычных клиентов. Тестовая точка.",
      },
      {
        id: "place_5",
        name: "Кальянная «Doha Lounge»",
        category: "fun",
        lat: 35.5384,   // Пусан
        lng: 129.3114,
        city: "Пусан",
        address: "Район Хэундэ, пример",
        description: "Кальян, чай, настолки. Тестовая точка.",
      },
    ];

    // Здесь будем хранить маркеры по id, чтобы потом фильтровать/прятать
    const markersById = {};

    window.addEventListener('load', function () {
      console.log("window loaded");

      if (!window.kakao || !kakao.maps) {
        console.error("Kakao SDK не загрузился. Проверь домен и appkey.");
        return;
      }

      const container = document.getElementById('map');

      const options = {
        center: new kakao.maps.LatLng(36.5, 127.8), // центр Кореи
        level: 11,
      };

      const map = new kakao.maps.Map(container, options);

      console.log("Карта создана, добавляем маркеры...");

      // ====== СОЗДАНИЕ МАРКЕРОВ И INFOWINDOW ======
      PLACES.forEach(place => {
        const position = new kakao.maps.LatLng(place.lat, place.lng);

        const marker = new kakao.maps.Marker({
          position,
          map, // сразу показываем на карте
        });

        markersById[place.id] = marker;

        // Простейший инфо-балун (потом заменим на красивую карточку)
        const content = `
          <div style="
            padding:8px 10px;
            font-size:13px;
            max-width:220px;
          ">
            <b>${place.name}</b><br/>
            <span style="color:#6b7280">${place.city}</span><br/>
            <span>${place.address}</span><br/>
            <span style="color:#9ca3af; font-size:12px;">${place.description}</span>
          </div>
        `;

        const infoWindow = new kakao.maps.InfoWindow({
          content,
          removable: true,
        });

        kakao.maps.event.addListener(marker, 'click', function () {
          infoWindow.open(map, marker);
        });
      });

      console.log("Маркеры загружены:", Object.keys(markersById).length);
    });
  </script>

</body>
</html>
