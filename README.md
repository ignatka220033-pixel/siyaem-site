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

  <!-- НОВЫЙ JavaScript KEY -->
  <script src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=5bb3be265d3f64faa0ba9d0b6928b2fb"></script>
</head>
<body>

  <div id="map"></div>

  <script>
    window.addEventListener('load', function () {
      console.log("window loaded");

      if (!window.kakao) {
        console.error("window.kakao отсутствует. SDK Kakao не загрузился. Проверь домен и appkey.");
        return;
      }
      if (!window.kakao.maps) {
        console.error("window.kakao есть, но kakao.maps нет. Проверь, что включён продукт Kakao Map.");
        return;
      }

      var container = document.getElementById('map');

      var options = {
        center: new window.kakao.maps.LatLng(36.5, 127.8),
        level: 12
      };

      var map = new window.kakao.maps.Map(container, options);

      var markerPosition = new window.kakao.maps.LatLng(36.78, 127.02);
      var marker = new window.kakao.maps.Marker({
        position: markerPosition
      });
      marker.setMap(map);

      console.log("Карта успешно создана");
    });
  </script>

</body>
</html>
