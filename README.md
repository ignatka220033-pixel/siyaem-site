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
    }
    #map {
      width: 100%;
      height: 100%;
    }
  </style>

  <!-- Подключаем Kakao Maps SDK
       ВАЖНО: тут должен быть JavaScript Key -->
  <script src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=f63924b38de08f3162d1ea0a73766b9a"></script>
</head>
<body>

  <!-- Контейнер для карты -->
  <div id="map"></div>

  <!-- Наш код, выполняется после подключения SDK -->
  <script>
    // Этот код выполнится, когда HTML уже загружен
    window.onload = function () {
      // Проверим, что объект kakao существует
      if (!window.kakao || !kakao.maps) {
        console.error("Kakao SDK не загрузился");
        return;
      }

      // Элемент карты
      var container = document.getElementById('map');

      // Центр карты — примерно центр Южной Кореи
      var options = {
        center: new kakao.maps.LatLng(36.5, 127.8),
        level: 12
      };

      // Создаём карту
      var map = new kakao.maps.Map(container, options);

      // Тестовый маркер (примерно Асан/Чхонан)
      var markerPosition = new kakao.maps.LatLng(36.78, 127.02);
      var marker = new kakao.maps.Marker({
        position: markerPosition
      });
      marker.setMap(map);

      console.log("Карта успешно создана");
    };
  </script>

</body>
</html>
