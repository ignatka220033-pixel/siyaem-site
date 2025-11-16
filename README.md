<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>SIYAEM KOREA MAP</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- KAKAO MAP SDK -->
<script src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=5bb3be265d3f64faa0ba9d0b6928b2fb"></script>

<style>
    body {
        margin: 0;
        padding: 0;
        height: 100vh;
        overflow: hidden;
        font-family: Arial;
        background: #020617;
        color: white;
    }

    /* ВЕРХНЯЯ ПАНЕЛЬ */
    .top-bar {
        width: 100%;
        height: 55px;
        background: rgba(0,0,0,0.35);
        backdrop-filter: blur(10px);
        display: flex;
        align-items: center;
        padding: 0 20px;
        justify-content: space-between;
        box-sizing: border-box;
        position: fixed;
        top: 0;
        z-index: 1000;
    }

    .logo {
        font-size: 22px;
        font-weight: 700;
        color: #38bdf8;
    }

    .top-buttons {
        display: flex;
        gap: 12px;
    }

    .btn {
        padding: 8px 16px;
        border-radius: 20px;
        background: #1e293b;
        cursor: pointer;
        transition: .2s;
    }

    .btn:hover {
        background: #334155;
    }

    .btn-sos {
        background: #ef4444;
        color: white;
        font-weight: bold;
    }

    /* КАРТА */
    #map {
        position: absolute;
        top: 55px;
        left: 0;
        width: 100%;
        height: calc(100vh - 55px);
    }

    /* ЛЕВАЯ ШТОРКА (МЕСТА) */
    .left-panel {
        position: fixed;
        top: 55px;
        left: -350px;
        width: 350px;
        height: calc(100vh - 55px);
        background: rgba(15,23,42,0.92);
        backdrop-filter: blur(12px);
        padding: 20px;
        transition: .4s ease;
        overflow-y: auto;
        z-index: 1500;
        color: white;
    }

    .left-panel.open {
        left: 0;
    }

    /* КНОПКА ОТКРЫТИЯ СЛЕВА */
    .open-left {
        position: fixed;
        top: 65px;
        left: 10px;
        z-index: 1600;
        padding: 10px 18px;
        background: #1e293b;
        border-radius: 18px;
        cursor: pointer;
        transition: .2s;
    }

    .open-left:hover {
        background: #334155;
    }

    /* ПРАВАЯ ШТОРКА (АККАУНТ) */
    .right-panel {
        position: fixed;
        top: 55px;
        right: -350px;
        width: 350px;
        height: calc(100vh - 55px);
        background: rgba(15,23,42,0.92);
        backdrop-filter: blur(12px);
        padding: 20px;
        transition: .4s ease;
        overflow-y: auto;
        z-index: 1500;
        color: white;
    }

    .right-panel.open {
        right: 0;
    }

    .open-right {
        position: fixed;
        top: 65px;
        right: 10px;
        z-index: 1600;
        padding: 10px 18px;
        background: #1e293b;
        border-radius: 18px;
        cursor: pointer;
    }

    .open-right:hover {
        background: #334155;
    }
</style>
</head>

<body>

<!-- ВЕРХНЯЯ ПАНЕЛЬ -->
<div class="top-bar">
    <div class="logo">SIYAEM KOREA</div>
    <div class="top-buttons">
        <div class="btn-sos">SOS — помощь</div>
    </div>
</div>

<!-- КАРТА -->
<div id="map"></div>

<!-- КНОПКИ ШТОРОК -->
<div class="open-left">Места</div>
<div class="open-right">Аккаунт</div>

<!-- ЛЕВАЯ ПАНЕЛЬ -->
<div class="left-panel" id="leftPanel">
    <h2>Места</h2>
    <p>Кафе “Сибирь”, Магазин “Балтика”, Автосервис “Prestige”, Moscow Style, Doha Lounge</p>
</div>

<!-- ПРАВАЯ ПАНЕЛЬ -->
<div class="right-panel" id="rightPanel">
    <h2>Аккаунт</h2>
    <p>Скоро здесь будет авторизация.</p>
</div>

<script>
/* --- ИНИЦИАЛИЗАЦИЯ КАРТЫ --- */
window.onload = function () {
    let mapContainer = document.getElementById('map');

    let options = {
        center: new kakao.maps.LatLng(37.5665, 126.9780), 
        level: 10
    };

    let map = new kakao.maps.Map(mapContainer, options);
};

/* --- ШТОРКИ --- */
document.querySelector(".open-left").onclick = function () {
    document.getElementById("leftPanel").classList.toggle("open");
};

document.querySelector(".open-right").onclick = function () {
    document.getElementById("rightPanel").classList.toggle("open");
};
</script>

</body>
</html>
