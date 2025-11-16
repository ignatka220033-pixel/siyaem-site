<!-- Контейнер карты -->
<div id="map" style="width:100%; height:100vh;"></div>

<!-- Подключение Kakao SDK -->
<script src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=5bb3be265d3f64faa0ba9d0b6928b2fb"></script>

<script>
window.onload = function () {

  if (!window.kakao || !kakao.maps) {
    console.error("Kakao SDK НЕ загрузился");
    return;
  }

  var container = document.getElementById("map");

  var options = {
    center: new kakao.maps.LatLng(36.5, 127.8),
    level: 12
  };

  var map = new kakao.maps.Map(container, options);

  var marker = new kakao.maps.Marker({
    position: new kakao.maps.LatLng(36.78, 127.01)
  });

  marker.setMap(map);
};
</script>
