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
    }

    /* карта должна занимать весь экран */
    #map {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 1;
    }

    /* шапка поверх карты */
    .top-banner {
      position: absolute;
      z-index: 2;
      top: 12px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(15, 23, 42, 0.9);
      padding: 6px 14px;
      border-radius: 999px;
      font-size: 13px;
      border: 1px solid rgba(148, 163, 184, 0.3);
      color: white;
      backdrop-filter: blur(10px);
    }
  </style>

  <!-- Kakao Map SDK c твоим ключом -->
  <script src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=5bb3be265d3f64faa0ba9d0b6928b2fb"></script>
</head>
<body>

  <!-- шапка -->
  <div class="top-banner">
    Siyaem Korea — тест русскоязычных мест на карте
  </div>

  <!-- контейнер карты -->
  <div id="map"></div>

  <script>
    const PLACES = [
      { id:"1", name:"Кафе «Сибирь»", category:"food", lat:37.5665, lng:126.9780 },
      { id:"2", name:"Магазин «Балтика»", category:"shop", lat:37.5162, lng:127.1002 },
      { id:"3", name:"Автосервис Prestige", category:"auto", lat:36.78, lng:127.02 },
      { id:"4", name:"Салон Moscow Style", category:"beauty", lat:35.1595, lng:126.8526 },
      { id:"5", name:"Кальянная Doha", category:"fun", lat:35.5384, lng:129.3114 },
    ];

    const markersById = {};

    window.onload = function () {
      console.log("window loaded");

      if (!window.kakao || !kakao.maps) {
        console.error("Kakao SDK не загрузился");
        return;
      }

      const container = document.getElementById("map");

      const options = {
        center: new kakao.maps.LatLng(36.5, 127.8),
        level: 11
      };

      const map = new kakao.maps.Map(container, options);

      console.log("Карта создана, добавляем маркеры...");

      PLACES.forEach(place => {
        const position = new kakao.maps.LatLng(place.lat, place.lng);

        const marker = new kakao.maps.Marker({
          position,
          map
        });

        markersById[place.id] = marker;

        const info = new kakao.maps.InfoWindow({
          content: `<div style="padding:10px;font-size:13px;">${place.name}</div>`
        });

        kakao.maps.event.addListener(marker, "click", () => {
          info.open(map, marker);
        });
      });

      console.log("Маркеры загружены:", PLACES.length);
    };
  </script>

</body>
</html>
