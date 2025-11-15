<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Siyaem Korea</title>

<!-- Leaflet карта -->
<link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>

<style>
/* === ОСНОВНОЙ ДИЗАЙН === */
body {
    margin: 0;
    padding: 0;
    background: #0f1120;
    color: #fff;
    font-family: Arial, sans-serif;
}

/* Верхняя панель */
.header {
    width: 100%;
    padding: 15px 20px;
    background: #14172b;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.header-title {
    font-size: 22px;
    font-weight: 700;
}

/* Кнопки в шапке */
.header-buttons button {
    background: #2c2f4a;
    color: #a4cfff;
    padding: 10px 15px;
    border: 0;
    margin-left: 10px;
    border-radius: 8px;
    cursor: pointer;
}

/* Контейнер */
.main {
    padding: 20px;
}

/* Категории */
.categories {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
}

.category {
    background: #1b1f33;
    padding: 10px 18px;
    border-radius: 10px;
    color: #9cd8ff;
    cursor: pointer;
    border: 1px solid #233;
}

/* Карта */
#map {
    width: 100%;
    height: 500px;
    margin-top: 20px;
    border-radius: 15px;
    overflow: hidden;
    border: 2px solid #2d3150;
}

/* Bottom buttons */
.sos-button, .partner-button {
    width: 100%;
    background: #2e3559;
    color: #d8e9ff;
    padding: 15px;
    border: 0;
    border-radius: 12px;
    margin-top: 15px;
    font-size: 18px;
    cursor: pointer;
}
</style>

</head>
<body>

<!-- ШАПКА -->
<div class="header">
    <div class="header-title">Siyaem Korea</div>

    <div class="header-buttons">
        <button onclick="openCabinet()">Личный кабинет</button>
        <button onclick="toggleTheme()">Светлая / Тёмная</button>
    </div>
</div>

<!-- ОСНОВНОЙ ЭКРАН -->
<div class="main">

    <h2>Один сервис. Большие возможности.</h2>
    <p style="color:#9bb8ff;">
        Показываем только проверенные русскоязычные места Кореи. 
        Помогаем партнёрам зарабатывать, а гостям — находить своё.
    </p>

    <!-- Категории -->
    <div class="categories">
        <div class="category">Еда</div>
        <div class="category">Салоны</div>
        <div class="category">Авто</div>
        <div class="category">Образование</div>
        <div class="category">Развлечения</div>
        <div class="category">Показать на карте</div>
        <div class="category">Оставить заявку</div>
    </div>

    <!-- КАРТА -->
    <div id="map"></div>

    <!-- Кнопки под картой -->
    <button class="partner-button" onclick="openPartner()">Стать партнёром</button>
    <button class="sos-button" onclick="openSOS()">SOS — Экстренная помощь</button>

</div>

<script>
/* === ИНИЦИАЛИЗАЦИЯ КАРТЫ === */
var map = L.map('map').setView([36.5, 127.8], 7);

L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
    attribution: "&copy; OSM",
}).addTo(map);

/* === ФУНКЦИИ === */
function openCabinet() {
    alert("Окно входа появится тут — позже подключим Firebase.");
}

function toggleTheme() {
    alert("Будет переключение темы — пока макет.");
}

function openSOS() {
    alert("Экстренные номера:\n112 — полиция\n119 — пожарные / скорая\nВаш номер: добавить позже");
}

function openPartner() {
    alert("Форма заявки партнёра появится тут — позже подключим EmailJS / backend.");
}
</script>

</body>
</html>
