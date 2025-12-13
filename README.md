<header>
  <div class="rate">JPY→HKD 匯率：<span id="rate">載入中...</span></div>
</header>

<main>
  <section id="itinerary" class="active">...</section>
  <section id="map">...</section>
  <section id="budget">...</section>
  <section id="shopping">...</section>
</main>

<nav>
  <button class="active" data-view="itinerary">行程表</button>
  <button data-view="map">地圖</button>
  <button data-view="budget">預算表</button>
  <button data-view="shopping">購物表</button>
</nav>

<script>
  // 匯率
  loadRate();

  // Tab 切換
  document.querySelectorAll('nav button').forEach(btn=>{
    btn.addEventListener('click',()=>{
      document.querySelectorAll('main section').forEach(s=>s.classList.remove('active'));
      document.getElementById(btn.dataset.view).classList.add('active');
      document.querySelectorAll('nav button').forEach(b=>b.classList.remove('active'));
      btn.classList.add('active');
    });
  });
</script>
