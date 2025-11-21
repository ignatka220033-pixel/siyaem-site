<!-- ВСТАВЬ ЭТО В <head> СВОЕГО САЙТА -->
<style>
  /* Контейнер всего блока */
  #ethno-apps {
    padding: 40px 20px;
  }

  #ethno-apps-title {
    text-align: center;
    margin-bottom: 24px;
    font-size: 24px;
    font-weight: 600;
  }

  .ethno-apps-wrapper {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    justify-content: center;
  }

  .ethno-phone {
    width: 320px;
    height: 640px;
    background: #ffffff;
    border-radius: 28px;
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.12);
    padding: 14px;
    display: flex;
    flex-direction: column;
    overflow: hidden;
  }

  .ethno-phone-header {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 6px 10px 10px;
    border-bottom: 1px solid #eee;
    font-size: 16px;
    font-weight: 600;
  }

  .ethno-phone-header span.ethno-sub {
    opacity: 0.7;
    font-size: 12px;
    margin-left: 6px;
    font-weight: 400;
  }

  .ethno-search-bar {
    margin: 10px 0;
    padding: 8px 10px;
    border-radius: 12px;
    border: 1px solid #ddd;
    font-size: 13px;
    color: #777;
    background: #fafafa;
  }

  .ethno-chips {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    margin-bottom: 10px;
  }

  .ethno-chip {
    padding: 4px 10px;
    border-radius: 999px;
    font-size: 11px;
    background: #f1f1f5;
    color: #333;
    border: 1px solid #e0e0e8;
    white-space: nowrap;
  }

  .ethno-map {
    flex: 1;
    border-radius: 14px;
    background: linear-gradient(135deg, #e6f2ff, #fef6e8);
    position: relative;
    margin-bottom: 10px;
    overflow: hidden;
  }

  .ethno-map-pin {
    position: absolute;
    width: 18px;
    height: 18px;
    border-radius: 50%;
    background: #ff5a5f;
    border: 2px solid #fff;
  }

  .ethno-map-pin:nth-child(1) { top: 20%; left: 30%; }
  .ethno-map-pin:nth-child(2) { top: 55%; left: 60%; }
  .ethno-map-pin:nth-child(3) { top: 70%; left: 20%; }

  .ethno-list {
    background: #fff;
    border-radius: 14px;
    border: 1px solid #eee;
    padding: 8px;
    margin-bottom: 10px;
    max-height: 140px;
    overflow: auto;
  }

  .ethno-list-item {
    padding: 6px 4px;
    border-bottom: 1px solid #f1f1f5;
    font-size: 12px;
  }

  .ethno-list-item:last-child {
    border-bottom: none;
  }

  .ethno-list-title-row {
    display: flex;
    justify-content: space-between;
    font-weight: 600;
    margin-bottom: 2px;
  }

  .ethno-list-sub {
    font-size: 11px;
    color: #777;
  }

  .ethno-badge {
    font-size: 10px;
    padding: 2px 6px;
    border-radius: 999px;
    background: #fce9eb;
    color: #c5404c;
    margin-left: 4px;
  }

  .ethno-nav-bar {
    display: flex;
    justify-content: space-around;
    align-items: center;
    padding-top: 6px;
    border-top: 1px solid #eee;
    font-size: 11px;
    color: #666;
  }

  .ethno-nav-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2px;
    flex: 1;
  }

  .ethno-nav-item.ethno-active {
    color: #111;
    font-weight: 600;
  }

  .ethno-dot {
    width: 4px;
    height: 4px;
    border-radius: 50%;
    background: #111;
  }

  /* Доп. теги для ETHNO WORK */
  .ethno-tag-row {
    display: flex;
    gap: 4px;
    margin-top: 2px;
    flex-wrap: wrap;
  }

  .ethno-tag {
    font-size: 9px;
    padding: 2px 6px;
    border-radius: 6px;
    border: 1px solid #e0e0e8;
    color: #555;
  }

  /* Метро */
  .ethno-metro-lines {
    display: flex;
    gap: 4px;
    margin-top: 6px;
    margin-bottom: 4px;
  }

  .ethno-metro-line {
    flex: 1;
    height: 4px;
    border-radius: 999px;
    background: #4b9cff;
  }

  .ethno-metro-line:nth-child(2) {
    background: #4cd964;
  }

  .ethno-metro-line:nth-child(3) {
    background: #ffcc00;
  }

  .ethno-route-box {
    border-radius: 10px;
    border: 1px solid #eee;
    padding: 6px 8px;
    font-size: 11px;
    margin-bottom: 6px;
    background: #fafafa;
  }

  .ethno-route-main {
    display: flex;
    justify-content: space-between;
    margin-bottom: 2px;
    font-weight: 600;
  }

  .ethno-route-sub {
    font-size: 10px;
    color: #777;
  }

  .ethno-btn {
    display: inline-block;
    padding: 4px 10px;
    border-radius: 999px;
    font-size: 11px;
    border: 1px solid #111;
    margin-top: 4px;
    background: #fff;
  }

  /* Адаптив под меньшие экраны */
  @media (max-width: 1024px) {
    #ethno-apps {
      padding: 20px 10px;
    }
  }
</style>

<!-- ВСТАВЬ ЭТО В <body> ГДЕ НУЖНО ПОКАЗАТЬ ТРИ ПРИЛОЖЕНИЯ -->
<section id="ethno-apps">
  <h2 id="ethno-apps-title">Экосистема ETHNOGRAM — три приложения</h2>

  <div class="ethno-apps-wrapper">
    <!-- ETHNOGRAM -->
    <div class="ethno-phone">
      <div class="ethno-phone-header">
        ETHNOGRAM <span class="ethno-sub">ядро</span>
      </div>

      <div class="ethno-search-bar">🔍 Поиск места, заведения...</div>

      <div class="ethno-chips">
        <div class="ethno-chip">☕ Кафе</div>
        <div class="ethno-chip">💇 Красота</div>
        <div class="ethno-chip">🚗 Авто</div>
        <div class="ethno-chip">🛒 Магазины</div>
        <div class="ethno-chip">⚙ Услуги</div>
      </div>

      <div class="ethno-map">
        <div class="ethno-map-pin"></div>
        <div class="ethno-map-pin"></div>
        <div class="ethno-map-pin"></div>
      </div>

      <div class="ethno-list">
        <div class="ethno-list-item">
          <div class="ethno-list-title-row">
            <span>Banana Coffee</span>
            <span class="ethno-badge">🎁 −5%</span>
          </div>
          <div class="ethno-list-sub">Сеул, Хондэ · 300 м</div>
        </div>
        <div class="ethno-list-item">
          <div class="ethno-list-title-row">
            <span>Русский салон</span>
          </div>
          <div class="ethno-list-sub">Асан, Дунпо · 1.2 км</div>
        </div>
        <div class="ethno-list-item">
          <div class="ethno-list-title-row">
            <span>Автосервис CLS</span>
          </div>
          <div class="ethno-list-sub">Инчхон · 4.3 км</div>
        </div>
      </div>

      <div class="ethno-nav-bar">
        <div class="ethno-nav-item ethno-active">
          <span>🗺️</span>
          <span>Карта</span>
          <div class="ethno-dot"></div>
        </div>
        <div class="ethno-nav-item">
          <span>🎁</span>
          <span>Подарки</span>
        </div>
        <div class="ethno-nav-item">
          <span>💼</span>
          <span>Работа</span>
        </div>
        <div class="ethno-nav-item">
          <span>🚇</span>
          <span>Маршруты</span>
        </div>
        <div class="ethno-nav-item">
          <span>👤</span>
          <span>Профиль</span>
        </div>
      </div>
    </div>

    <!-- ETHNO WORK -->
    <div class="ethno-phone">
      <div class="ethno-phone-header">
        ETHNO WORK <span class="ethno-sub">работа</span>
      </div>

      <div class="ethno-search-bar">🔍 Поиск вакансий...</div>

      <div class="ethno-chips">
        <div class="ethno-chip">🏙️ Сеул</div>
        <div class="ethno-chip">🏠 С жильём</div>
        <div class="ethno-chip">💬 Без языка</div>
        <div class="ethno-chip">💰 Почасовая</div>
      </div>

      <div class="ethno-list" style="flex: 1; max-height: none;">
        <div class="ethno-list-item">
          <div class="ethno-list-title-row">
            <span>Бариста в кафе</span>
            <span>12 000₩/ч</span>
          </div>
          <div class="ethno-list-sub">Хондэ · смены 6–8 ч</div>
          <div class="ethno-tag-row">
            <div class="ethno-tag">С жильём</div>
            <div class="ethno-tag">Без корейского</div>
          </div>
        </div>
        <div class="ethno-list-item">
          <div class="ethno-list-title-row">
            <span>Склад / упаковка</span>
            <span>11 000₩/ч</span>
          </div>
          <div class="ethno-list-sub">Инчхон · ночь</div>
          <div class="ethno-tag-row">
            <div class="ethno-tag">Только мужчины</div>
          </div>
        </div>
        <div class="ethno-list-item">
          <div class="ethno-list-title-row">
            <span>Мойка авто</span>
            <span>협의</span>
          </div>
          <div class="ethno-list-sub">Асан · дневные смены</div>
          <div class="ethno-tag-row">
            <div class="ethno-tag">Бонусы</div>
            <div class="ethno-tag">Опыт приветствуется</div>
          </div>
        </div>
      </div>

      <div class="ethno-nav-bar">
        <div class="ethno-nav-item ethno-active">
          <span>📄</span>
          <span>Вакансии</span>
          <div class="ethno-dot"></div>
        </div>
        <div class="ethno-nav-item">
          <span>🗺️</span>
          <span>Карта</span>
        </div>
        <div class="ethno-nav-item">
          <span>📩</span>
          <span>Отклики</span>
        </div>
        <div class="ethno-nav-item">
          <span>👤</span>
          <span>Профиль</span>
        </div>
      </div>
    </div>

    <!-- ETHNO METRO -->
    <div class="ethno-phone">
      <div class="ethno-phone-header">
        ETHNO METRO <span class="ethno-sub">маршруты</span>
      </div>

      <div class="ethno-search-bar">출발 → 도착 (откуда → куда)</div>

      <div class="ethno-metro-lines">
        <div class="ethno-metro-line"></div>
        <div class="ethno-metro-line"></div>
        <div class="ethno-metro-line"></div>
      </div>

      <div class="ethno-route-box">
        <div class="ethno-route-main">
          <span>홍대입구 → 강남</span>
          <span>32 мин</span>
        </div>
        <div class="ethno-route-sub">2 пересадки · T-money</div>
        <span class="ethno-btn">Показать маршрут</span>
      </div>

      <div class="ethno-route-box">
        <div class="ethno-route-main">
          <span>아산 → 서울역</span>
          <span>1 ч 10 мин</span>
        </div>
        <div class="ethno-route-sub">KTX + метро</div>
        <span class="ethno-btn">Как добраться</span>
      </div>

      <div class="ethno-list" style="flex: 1;">
        <div class="ethno-list-item">
          <div class="ethno-list-title-row">
            <span>🚇 Карта метро</span>
          </div>
          <div class="ethno-list-sub">Линии, станции, пересадки</div>
        </div>
        <div class="ethno-list-item">
          <div class="ethno-list-title-row">
            <span>🚕 Такси</span>
          </div>
          <div class="ethno-list-sub">Открыть KakaoTaxi</div>
        </div>
        <div class="ethno-list-item">
          <div class="ethno-list-title-row">
            <span>📍 Места рядом</span>
          </div>
          <div class="ethno-list-sub">Открыть в Ethnogram</div>
        </div>
      </div>

      <div class="ethno-nav-bar">
        <div class="ethno-nav-item ethno-active">
          <span>🧭</span>
          <span>Маршруты</span>
          <div class="ethno-dot"></div>
        </div>
        <div class="ethno-nav-item">
          <span>🚇</span>
          <span>Карта</span>
        </div>
        <div class="ethno-nav-item">
          <span>🚕</span>
          <span>Такси</span>
        </div>
        <div class="ethno-nav-item">
          <span>👤</span>
          <span>Профиль</span>
        </div>
      </div>
    </div>
  </div>
</section>
