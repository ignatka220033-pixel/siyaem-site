<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Siyaem Korea Map</title>

<!-- KAKAO SDK -->
<script src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=5bb3be265d3f64faa0ba9d0b6928b2fb"></script>

<style>
  body {
    margin: 0;
    background: #020617;
    font-family: Arial, sans-serif;
    padding: 8px;
    color: white;
  }

  /* Широкий контейнер – на всю ширину */
  .container-wide {
    width: 100%;
    max-width: none;
    margin: 0;
  }

  /* Шапка */
  .header {
    padding: 10px;
  }

  /* Поиск */
  .search-box input {
    width: 100%;
    padding: 12px;
    border-radius: 12px;
    border: none;
    font-size: 16px;
    outline: none;
    background: #0f172a;
    color: #fff;
  }

  /* Основной макет */
  .layout {
    width: 100%;
    display: flex;
    gap: 12px;
    height: calc(100vh - 120px);
  }

  /* Левая панель */
  .left-panel {
    width: 28%;
    background: #0f172a;
    border-radius: 14px;
    padding: 14px;
    overflow-y: auto;
  }

  /* Карта справа */
  #map {
    flex: 1;
    height: 100%;
    border-radius: 14px;
    overflow: hidden;
  }

  .place-box {
    margin-top: 12px;
    padding: 12px;
    border-radius: 12px;
    background: #1e293b;
  }

  .pill {
    padding: 6px 14px;
    border-radius: 20px;
    background: #1e293b;
    display: inline-block;
    margin-right: 6px;
    cursor: pointer;
  }
</style>
</head>

<body>
<div class="container-wide">

  <!-- HEADER -->
  <div class="header">
    <h1>SIYAEM KOREA</h1>
  </div>

  <!-- SEARCH -->
  <div class="search-box">
    <input type="text" placeholder="Поиск пока не подключён..." />
  </div>

  <!-- MAIN LAYOUT -->
  <div class="layout">
    
    <!-- LEFT PANEL -->
    <div class="left-panel">
      <h3>Места поблизости</h3>

      <div>
        <span class="pill">Все</span>
        <span class="pill">Еда</span>
        <span class="pill">Магазины</span>
        <span class="pill">Авто</span>
        <span class="pill">Салоны</span>
        <span class="pill">Развлечения</span>
      </div>

      <!-- МЕСТА -->
      <div class="place-box">
        <b>Кафе «Сибирь»</b><br>
        Сеул • Еда<br>
        Домашние пельмени, борщ и компоты.
      </div>

      <div class="place-box">
        <b>Магазин «Балтика»</b><br>
        Сеул • Магазин<br>
        Русские продукты, сладости и консервы.
      </div>

      <div class="place-box">
        <b>Автосервис «Prestige»</b><br>
        Асан • Авто<br>
        Русскоязычный сервис + помощь с ремонтом.
      </div>

      <div class="place-box">
        <b>Салон красоты «Moscow Style»</b><br>
        Сувон • Красота<br>
        Окрашивание, уход, стрижки.
      </div>

      <div class="place-box">
        <b>Кальянная «Doha Lounge»</b><br>
        Пусан • Развлечения<br>
        Настольные игры, трансляции и кальян.
      </div>

    </div>

    <!-- MAP -->
    <div id="map"></div>

  </div>
</div>

<script>
// Ожидание загрузки окна
window.onload = function () {
  console.log("window loaded");

  try {
    if (!window.kakao || !kakao.maps) {
      console.error("Kakao SDK не загрузился!");
      return;
    }

    let container = document.getElementById("map");

    let options = {
      center: new kakao.maps.LatLng(36.5, 127.9),
      level: 12
    };

    let map = new kakao.maps.Map(container, options);
    console.log("Карта успешно создана");

    // Маркеры
    let places = [
      { name: "Кафе Сибирь", lat: 37.550, lng: 126.990 },
      { name: "Балтика", lat: 37.540, lng: 127.020 },
      { name: "Prestige", lat: 36.780, lng: 127.010 },
      { name: "Moscow Style", lat: 37.260, lng: 127.010 },
      { name: "Doha Lounge", lat: 35.160, lng: 129.060 }
    ];

    for (let p of places) {
      new kakao.maps.Marker({
        map: map,
        position: new kakao.maps.LatLng(p.lat, p.lng)
      });
    }

    console.log("Маркеры загружены:", places.length);

  } catch (e) {
    console.error("Ошибка карты:", e);
  }
};
</script>

</body>
</html>
