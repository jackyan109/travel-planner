<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
  <title>旅行 App 模擬</title>
  <style>
    body {
      margin: 0;
      font-family: "Noto Sans TC", Arial, sans-serif;
      background: url('https://images.unsplash.com/photo-1549692520-acc6669e2f0c?q=80&w=1400&auto=format&fit=crop') no-repeat center center fixed;
      background-size: cover;
      color: #fff;
    }
    .overlay {
      position: fixed; inset: 0;
      background: rgba(0,0,0,0.6);
      z-index: -1;
    }
    header {
      padding: 10px;
      font-size: 14px;
      background: rgba(0,0,0,0.5);
      position: sticky;
      top: 0;
    }
    header .rate {
      font-weight: bold;
    }
    main {
      padding: 15px;
    }
    section {
      display: none;
      background: rgba(0,0,0,0.5);
      border-radius: 10px;
      padding: 15px;
    }
    section.active { display: block; }
    nav {
      position: fixed; bottom: 0; left: 0; right: 0;
      display: flex;
      background: rgba(0,0,0,0.8);
    }
    nav button {
      flex: 1;
      padding: 12px;
      border: none;
      background: transparent;
      color: #fff;
      font-size: 14px;
      cursor: pointer;
    }
    nav button.active {
      background: rgba(255,255,255,0.2);
    }
    input, textarea {
      width: 100%;
      padding: 8px;
      margin: 5px 0;
      border-radius: 6px;
      border: none;
    }
    .list { margin-top: 10px; }
    .item {
      background: rgba(255,255,255,0.1);
      padding: 8px;
      border-radius: 6px;
      margin-bottom: 6px;
    }
    .thumbs img {
      width: 100px; height: 100px; object-fit: cover;
      margin: 5px; border-radius: 8px;
    }
  </style>
</head>
<body>
  <div class="overlay"></div>
  <header>
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
      <button onclick="addItinerary()">新增行程</button>
      <div id="it_list" class="list"></div>
    </section>

    <!-- 地圖 -->
    <section id="map">
      <h2>地圖</h2>
      <select id="map_select"></select>
      <iframe id="map_iframe" width="100%" height="300"></iframe>
    </section>

    <!-- 預算表 -->
    <section id="budget">
      <h2>預算表</h2>
      <input id="bd_title" placeholder="項目">
      <input id="bd_amount" type="number" placeholder="金額 (JPY)">
      <button onclick="addBudget()">新增項目</button>
      <div id="bd_list" class="list"></div>
      <div>合計：<span id="bd_sum">¥0 → HK$0</span></div>
    </section>

    <!-- 購物表 -->
    <section id="shopping">
      <h2>購物表</h2>
      <input id="sp_title" placeholder="品項">
      <input id="sp_price" type="number" placeholder="價格 (JPY)">
      <input id="sp_photo" type="file" accept="image/*">
      <button onclick="addShopping()">新增品項</button>
      <div id="sp_list" class="list"></div>
      <div class="thumbs" id="sp_thumbs"></div>
    </section>
  </main>

  <nav>
    <button class="active" onclick="show('itinerary')">行程表</button>
    <button onclick="show('map')">地圖</button>
    <button onclick="show('budget')">預算表</button>
    <button onclick="show('shopping')">購物表</button>
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
    function show(id){
      document.querySelectorAll('section').forEach(s=>s.classList.remove('active'));
      document.getElementById(id).classList.add('active');
      document.querySelectorAll('nav button').forEach(b=>b.classList.remove('active'));
      document.querySelector(`nav button[onclick="show('${id}')"]`).classList.add('active');
    }

    // 行程表
    const itinerary = [];
    function addItinerary(){
      const title=document.getElementById('it_title').value;
      const date=document.getElementById('it_date').value;
      const time=document.getElementById('it_time').value;
      const place=document.getElementById('it_place').value;
      itinerary.push({title,date,time,place});
      renderItinerary();
    }
    function renderItinerary(){
      const list=document.getElementById('it_list');
      list.innerHTML='';
      itinerary.forEach((it,i)=>{
        const div=document.createElement('div');
        div.className='item';
        div.textContent=`${it.date} ${it.time} - ${it.title} @ ${it.place}`;
        list.appendChild(div);
      });
      const sel=document.getElementById('map_select');
      sel.innerHTML='';
      itinerary.forEach(it=>{
        const opt=document.createElement('option');
        opt.value=it.place;
        opt.textContent=it.title;
        sel.appendChild(opt);
      });
    }

    // 地圖
    document.getElementById('map_select').addEventListener('change',e=>{
      const place=e.target.value;
      document.getElementById('map_iframe').src=`https://www.google.com/maps?q=${encodeURIComponent(place)}&output=embed`;
    });

    // 預算表
    const budget=[];
    function addBudget(){
      const title=document.getElementById('bd_title').value;
      const amount=Number(document.getElementById('bd_amount').value);
      budget.push({title,amount});
      renderBudget();
    }
    function renderBudget(){
      const list=document.getElementById('bd_list');
      list.innerHTML='';
      let sum=0;
      budget.forEach(b=>{
        const div=document.createElement('div');
        div.className='item';
        div.textContent=`${b.title}: ¥${b.amount}`;
        list.appendChild(div);
        sum+=b.amount;
      });
      const hkd=sum*(window.jpyRate||0.055);
      document.getElementById('bd_sum').textContent=`¥${sum} → HK$${hkd.toFixed(2)}`;
    }

    // 購物表
    const shopping=[];
    function addShopping(){
      const title=document.getElementById('sp_title').value;
      const price=document.getElementById('sp_price').value;
      const file=document.getElementById('sp_photo').files[0];
      const reader=new FileReader();
      reader.onload=function(e){
        shopping.push({title,price,photo:e.target.result});
        renderShopping();
      };
      if(file){ reader.readAsDataURL(file); } else {
        shopping.push({title,price});
        renderShopping();
      }
    }
    function renderShopping(){
      const list=document.getElementById('sp_list');
      const thumbs=document.getElementById('sp_thumbs');
      list.innerHTML=''; thumbs
