<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <title>Siyaem Korea Map</title>

    <style>
        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            background: #0b1c2d;
            font-family: Arial, sans-serif;
        }

        #map {
            width: 100%;
            height: 100%;
        }

        /* Верхняя панель */
        .top-bar {
            position: absolute;
            top: 10px;
            left: 50%;
            transform: translateX(-50%);
            width: 80%;
            height: 45px;
            background: rgba(255,255,255,0.07);
            border-radius: 15px;
            backdrop-filter: blur(10px);
            display: flex;
            align-items: center;
            padding: 0 15px;
            color: #fff;
            z-index: 10;
        }

        .logo {
            font-weight: bold;
            font-size: 20px;
            margin-right: 20px;
        }
    </style>

    <!-- Подключаем Kakao Maps SDK -->
    <script type="text/javascript" src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=f63924b38de08f3162d1ea0a73766b9a"></script>

</head>
<body>

    <div class="top-bar">
        <div class="logo">SIYAEM KOREA</div>
        Карта русскоязычных мест
    </div>

    <div id="map"></div>

    <script>
        // Создаём карту
        var mapContainer = document.getElementById('map');

        var mapOptions = {
            center: new kakao.maps.LatLng(36.5, 127.8), // центр Кореи
            level: 13 // масштаб
        };

        var map = new kakao.maps.Map(mapContainer, mapOptions);

        console.log("Kakao карта успешно загружена!");
    </script>

</body>
</html>
