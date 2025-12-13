<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1" />
  <title>旅行 App 模擬</title>
  <style>
    body {
      margin: 0;
      font-family: "Noto Sans TC", Arial, sans-serif;
      background: url('https://d1grca2t3zpuug.cloudfront.net/2017/06/fujimt016-1750989984.webp') center/cover fixed no-repeat;
      color: #fff;
    }
    .overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.6); z-index: -1; }
    header {
      padding: 10px;
      font-size: 14px;
      background: rgba(0,0,0,0.5);
      position: sticky;
      top: 0;
    }
    .subtitle { font-size: 18px; font-weight: 700; margin-bottom: 6px; }
    header .rate { font-weight: bold; }
    main { padding: 15px; }
    section { display: none; background: rgba(0,0,0,0.5); border-radius: 10px; padding: 15px; }
    section.active { display: block; }
    nav { position: fixed; bottom: 0; left: 0; right: 0; display: flex; background: rgba(0,0,0,0.8); }
    nav button { flex: 1; padding: 12px; border: none; background: transparent; color: #fff; font-size: 14px; cursor: pointer; }
    nav button.active { background: rgba(255,255,255,0.2); }
    input { width: 100%; padding: 8px; margin: 5px 0; border-radius: 6px; border: none; }
    .list { margin-top: 10px; }
    .item { background: rgba(255,255,255,0.1); padding: 8px; border-radius: 6px; margin-bottom: 6px; display:flex; justify-content:space-between; align-items:center; }
    .thumbs img { width: 100px; height: 100px; object-fit: cover; margin: 5px; border-radius: 8px; }
  </style>
</head>
<body>
  <div class="overlay"></div>
  <header>
    <div class="subtitle">🇯🇵 東京2025-2026</div>
    <div class="rate">JPY→HKD 匯率：<span id="rate">載入中...</span></div>
  </header>

  <main>
    <!-- 行程表 -->
    <section id="itinerary" class="active">
      <h2>行程表</h2>
      <input id="it_title" placeholder="行程項目">
      <input id="it_date" type="date">
      <input id="it_time" type="time">
      <input id="it_place" placeholder="地點">
      <button id="it_add">新增行程</button>
      <div id="it_list"></div>
    </section>

    <!-- 地圖 -->
    <section id="map">
      <h2>地圖</h2>
      <label for="map_date">選擇日期</label>
      <select id="map_date"></select>
      <button id="map_open">在地圖 App 開啟</button>
      <iframe id="map_iframe" width="100%" height="300"></iframe>
    </section>

    <!-- 預算表 -->
    <section id="budget">
      <h2>預算表</h2>
      <input id="bd_title" placeholder="項目">
      <input id="bd_amount" type="number" placeholder="金額 (JPY)">
      <button id="bd_add">新增項目</button>
      <div id="bd_list"></div>
      <div>合計：<span id="bd_sum">¥0 → HK$0</span></div>
    </section>

    <!-- 購物表 -->
    <section id="shopping">
      <h2>購物表</h2>
      <input id="sp_title" placeholder="品項">
      <input id="sp_price" type="number" placeholder="價格 (JPY)">
      <input id="sp_photo" type="file" accept="image/*">
      <button id="sp_add">新增品項</button>
      <div id="sp_list"></div>
      <div class="thumbs" id="sp_thumbs"></div>
    </section>
  </main>

  <nav>
    <button class="active" data-view="itinerary">行程表</button>
    <button data-view="map">地圖</button>
    <button data-view="budget">預算表</button>
    <button data-view="shopping">購物表</button>
  </nav>

  <script>
    // 匯率
    async function loadRate(){
      try{
        const res = await fetch('https://api.exchangerate.host/latest?base=JPY&symbols=HKD');
        const data = await res.json();
        const rate = data.rates.HKD;
        document.getElementById('rate').textContent = rate.toFixed(4);
        window.jpyRate = rate;
      }catch(e){
        document.getElementById('rate').textContent = '請手動輸入';
        window.jpyRate = 0.055;
      }
    }
    loadRate();

    // Tab 切換
    document.querySelectorAll('nav button').forEach(btn=>{
      btn.addEventListener('click',()=>{
        document.querySelectorAll('section').forEach(s=>s.classList.remove('active'));
        document.getElementById(btn.dataset.view).classList.add('active');
        document.querySelectorAll('nav button').forEach(b=>b.classList.remove('active'));
        btn.classList.add('active');
      });
    });

    // 行程表
    const itinerary=[];
    document.getElementById('it_add').addEventListener('click',()=>{
      const title=document.getElementById('it_title').value;
      const date=document.getElementById('it_date').value;
      const time=document.getElementById('it_time').value;
      const place=document.getElementById('it_place').value;
      if(!title){alert("請輸入行程項目");return;}
      itinerary.push({title,date,time,place});
      renderItinerary();
      syncMapDates();
    });
    function renderItinerary(){
      const list=document.getElementById('it_list');
      list.innerHTML='';
      itinerary.forEach((it,index)=>{
        const div=document.createElement('div');
        div.className='item';
        div.innerHTML=`<span>${it.date} ${it.time} - ${it.title} @ ${it.place}</span>
                       <button onclick="deleteItinerary(${index})">刪除</button>`;
        list.appendChild(div);
      });
    }
    function deleteItinerary(index){
      itinerary.splice(index,1);
      renderItinerary();
      syncMapDates();
    }

    // 地圖依日期顯示路徑
    function syncMapDates(){
      const sel=document.getElementById('map_date');
      sel.innerHTML='';
      const dates=[...new Set(itinerary.map(it=>it.date).filter(Boolean))];
      if(!dates.length){
        const opt=document.createElement('option');
        opt.value=''; opt.textContent='請先新增行程';
        sel.appendChild(opt);
        document.getElementById('map_iframe').src='';
        return;
      }
      dates.forEach(d=>{
        const opt=document.createElement('option');
        opt.value=d; opt.textContent=d;
        sel.appendChild(opt);
      });
    }
    function setMapByDate(date){
      if(!date){document.getElementById('map_iframe').src='';return;}
      const places=itinerary.filter(it=>it.date===date).map(it=>it.place).filter(Boolean);
      if(!places.length){document.getElementById('map_iframe').src='';return;}
      const url='https://www.google.com/maps/dir/'+places.map(encodeURIComponent).join('/');
      document.getElementById('map_iframe').src=url;
    }
    document.getElementById('map_date').addEventListener('change',e=>setMapByDate(e.target.value));
    document.getElementById('map_open').addEventListener('click',()=>{
      const date=document.getElementById('map_date').value;
