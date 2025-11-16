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

  <!-- ВАЖНО: сюда вставляем JavaScript KEY от Kakao -->
  <script src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=f63924b38de08f3162d1ea0a73766b9a&autoload=false"></script>
</head>
<body>

  <div id="map"></div>

  <script>
    // Ждём, пока SDK загрузится
    kakao.maps.load(function () {
      var container = document.getElementById('map');

      // Центр — примерно середина Южной Кореи
      var options = {
        center: new kakao.maps.LatLng(36.5, 127.8),
        level: 12
      };

      var map = new kakao.maps.Map(container, options);

      // Тестовый маркер (где-то в районе Асан/Чхонан)
      var marker = new kakao.maps.Marker({
        position: new kakao.maps.LatLng(36.78, 127.02)
      });
      marker.setMap(map);

      console.log("Карта создана");
    });
  </script>

</body>
</html>
