<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1" />
  <title>旅行 App 模擬</title>
  <style>
    :root{
      --bg-overlay: rgba(0,0,0,0.60);
      --card: rgba(0,0,0,0.55);
      --text: #ffffff;
      --muted: #cfe1ff;
      --border: rgba(255,255,255,0.18);
      --accent: #4db5ff;
      --danger: #ff6b6b;
      --radius: 12px;
    }
    *{box-sizing:border-box}
    body{
      margin:0; color:var(--text);
      font-family:"Noto Sans TC", system-ui, -apple-system, "Segoe UI", Roboto, Arial, sans-serif;
      background: url('https://d1grca2t3zpuug.cloudfront.net/2017/06/fujimt016-1750989984.webp') center/cover fixed no-repeat;
    }
    .overlay{position:fixed;inset:0;background:var(--bg-overlay);z-index:-1}

    header{
      position:sticky; top:0; z-index:10;
      background: rgba(0,0,0,0.5);
      backdrop-filter: blur(6px);
      border-bottom: 1px solid var(--border);
      padding: 10px 14px;
      display:flex; align-items:center; justify-content:space-between;
    }
    .rate-wrap{display:flex; align-items:center; gap:10px; font-size:14px}
    .rate-value{font-weight:700}
    .rate-input{width:120px; padding:6px 8px; border-radius:8px; border:1px solid var(--border); background:rgba(255,255,255,0.1); color:var(--text)}
    .source{font-size:12px; color:var(--muted)}

    main{padding:14px 14px 72px}
    section{
      display:none;
      background:var(--card); border:1px solid var(--border); border-radius:var(--radius);
      padding:14px; margin-bottom:14px;
    }
    section.active{display:block}
    h2{margin:0 0 10px; font-size:16px}
    label{display:block; font-size:12px; color:var(--muted); margin:8px 0 6px}
    input, select{
      width:100%; padding:10px 12px; border-radius:10px;
      border:1px solid var(--border);
      background:rgba(255,255,255,0.08); color:var(--text); outline:none;
    }
    input:focus, select:focus{border-color:#8bd0ff; box-shadow:0 0 0 3px rgba(77,181,255,0.25)}
    .grid-2{display:grid; grid-template-columns:1fr 1fr; gap:10px}
    .actions{display:flex; gap:8px; flex-wrap:wrap; margin-top:10px}
    button{
      border:none; border-radius:10px; padding:10px 12px; font-weight:700; cursor:pointer;
    }
    .btn-primary{background:var(--accent); color:#00131e}
    .btn-secondary{background:rgba(255,255,255,0.12); color:var(--text); border:1px solid var(--border)}
    .btn-danger{background:var(--danger); color:#100505}
    .btn-ghost{background:transparent; color:var(--text); border:1px solid var(--border)}

    .list{display:grid; gap:10px; margin-top:10px}
    .item{
      display:grid; grid-template-columns:1fr auto; gap:10px; align-items:center;
      background:rgba(0,0,0,0.35); border:1px solid var(--border); border-radius:10px; padding:10px;
    }
    .meta{font-size:12px; color:var(--muted); margin-top:4px}
    .empty{text-align:center; color:var(--muted); padding:14px; border:1px dashed var(--border); border-radius:10px}

    iframe{width:100%; height:340px; border:0; border-radius:10px}

    .sumrow{display:flex; justify-content:space-between; align-items:center; gap:10px; margin-top:8px}
    .sum{font-weight:800}

    .thumbs{display:grid; grid-template-columns:repeat(3,1fr); gap:8px; margin-top:10px}
    .thumb{position:relative; border:1px solid var(--border); border-radius:10px; overflow:hidden}
    .thumb img{width:100%; height:100%; object-fit:cover; aspect-ratio:1/1}
    .thumb .del{position:absolute; right:6px; top:6px; padding:6px 8px; font-size:11px}

    nav{
      position:fixed; left:0; right:0; bottom:0; z-index:10;
      background: rgba(0,0,0,0.75); backdrop-filter: blur(6px);
      border-top:1px solid var(--border);
    }
    .tabs{display:grid; grid-template-columns:repeat(4,1fr)}
    .tab{
      display:flex; flex-direction:column; align-items:center; justify-content:center;
      padding:10px 8px; gap:4px; color:var(--muted); font-size:12px; background:transparent; border:none; cursor:pointer;
    }
    .tab.active{color:var(--text)}
  </style>
</head>
<body>
  <div class="overlay" aria-hidden="true"></div>

  <header>
    <div class="rate-wrap">
      <span>JPY→HKD 匯率：</span>
      <span id="rateValue" class="rate-value">載入中…</span>
      <input id="rateInput" class="rate-input" type="number" step="0.0001" min="0" placeholder="手動輸入匯率">
    </div>
    <div class="source">資料來源：市場匯率（可手動覆蓋）</div>
  </header>

  <main>
    <!-- 行程表 -->
    <section id="itinerary" class="active">
      <h2>行程表</h2>
      <div class="grid-2">
        <div>
          <label for="it_title">行程項目</label>
          <input id="it_title" placeholder="例如：富士山五合目 / 淺草寺" />
        </div>
        <div>
          <label for="it_place">地點</label>
          <input id="it_place" placeholder="地點（用於地圖）" />
        </div>
      </div>
      <div class="grid-2">
        <div>
          <label for="it_date">日期</label>
          <input id="it_date" type="date" />
        </div>
        <div>
          <label for="it_time">時間</label>
          <input id="it_time" type="time" />
        </div>
      </div>
      <div class="actions">
        <button id="it_add" class="btn-primary">新增行程</button>
        <button id="it_sort" class="btn-secondary">按日期時間排序</button>
        <button id="it_clear" class="btn-danger">清除全部</button>
      </div>
      <div id="it_list" class="list">
        <div class="empty">尚未加入任何行程。</div>
      </div>
    </section>

    <!-- 地圖 -->
    <section id="map">
      <h2>地圖</h2>
      <div class="grid-2">
        <div>
          <label for="map_select">選擇行程地點</label>
          <select id="map_select">
            <option value="">請先新增行程地點</option>
          </select>
        </div>
        <div style="align-self:end; text-align:right">
          <button id="map_open" class="btn-ghost">在地圖 App 開啟</button>
        </div>
      </div>
      <iframe id="map_iframe" title="地圖"></iframe>
    </section>

    <!-- 預算表 -->
    <section id="budget">
      <h2>預算表</h2>
      <div class="grid-2">
        <div>
          <label for="bd_title">項目</label>
          <input id="bd_title" placeholder="例如：交通 / 餐飲 / 門票" />
        </div>
        <div>
          <label for="bd_amount">金額（JPY）</label>
          <input id="bd_amount" type="number" min="0" step="1" placeholder="例如：1200" />
        </div>
      </div>
      <div class="actions">
        <button id="bd_add" class="btn-primary">新增項目</button>
        <button id="bd_clear" class="btn-danger">清除全部</button>
      </div>
      <div id="bd_list" class="list">
        <div class="empty">還沒有預算項目。</div>
      </div>
      <div class="sumrow">
        <div class="source">可調整上方匯率以更新 HKD</div>
        <div class="sum">
          合計：<span id="bd_sum_jpy">¥0</span> → <span id="bd_sum_hkd">HK$0.00</span>
        </div>
      </div>
    </section>

    <!-- 購物表 -->
    <section id="shopping">
      <h2>購物表</h2>
      <div class="grid-2">
        <div>
          <label for="sp_title">品項</label>
          <input id="sp_title" placeholder="例如：和菓子 / 手信 / 服飾" />
        </div>
        <div>
          <label for="sp_price">價格（JPY，可選）</label>
          <input id="sp_price" type="number" min="0" step="1" placeholder="例如：850" />
        </div>
      </div>
      <label for="sp_photo">照片（可選）</label>
      <input id="sp_photo" type="file" accept="image/*" />
      <div class="actions">
        <button id="sp_add" class="btn-primary">新增品項</button>
        <button id="sp_clear" class="btn-danger">清除全部</button>
      </div>
      <div id="sp_list" class="list">
        <div class="empty">尚未加入任何購物品項。</div>
      </div>
      <div id="sp_thumbs" class="thumbs"></div>
    </section>
  </main>

  <nav>
    <div class="tabs">
      <button class="tab active" data-view="itinerary">行程表</button>
      <button class="tab" data-view="map">地圖</button>
      <button class="tab" data-view="budget">預算表</button>
      <button class="tab" data-view="shopping">購物表</button>
    </div>
  </nav>

  <script>
    // Persistent state
    const KEY = 'travel_app_state_v2';
    const state = {
      rate: null,                // JPY -> HKD
      itinerary: [],             // {title,date,time,place}
      budget: [],                // {title,amountJPY}
      shopping: []               // {title,priceJPY?,photoDataURL?}
    };

    // Safe helpers
    const $ = (id)=>document.getElementById(id);

    // Load / Save
    function load(){
      try{
        const raw = localStorage.getItem(KEY);
        if(raw){
          const data = JSON.parse(raw);
          state.rate = typeof data.rate==='number' ? data.rate : null;
          state.itinerary = Array.isArray(data.itinerary)? data.itinerary : [];
          state.budget = Array.isArray(data.budget)? data.budget : [];
          state.shopping = Array.isArray(data.shopping)? data.shopping : [];
        }
      }catch(e){}
    }
    function save(){
      localStorage.setItem(KEY, JSON.stringify(state));
    }

    // Tabs
    function show(view){
      document.querySelectorAll('main section').forEach(s => s.classList.remove('active'));
      $(view).classList.add('active');
      document.querySelectorAll('.tab').forEach(b => b.classList.remove('active'));
      document.querySelector(`.tab[data-view="${view}"]`).classList.add('active');
    }

    // Exchange rate with retry + timeout + manual override
    async function fetchRateWithRetry(maxTry=2){
      const url = 'https://api.exchangerate.host/latest?base=JPY&symbols=HKD';
      for(let i=0;i<maxTry;i++){
        try{
          const ctrl = new AbortController();
          const timer = setTimeout(()=>ctrl.abort(), 4000); // 4s timeout
          const res = await fetch(url, {signal: ctrl.signal, cache:'no-store'});
          clearTimeout(timer);
          const data = await res.json();
          const rate = data?.rates?.HKD;
          if(typeof rate === 'number' && rate > 0){
            state.rate = rate;
            $('rateValue').textContent = rate.toFixed(4);
            $('rateInput').value = rate.toFixed(4);
            save(); updateBudgetSum();
            return;
          }
        }catch(e){
          // try next
        }
      }
      // Fallback
      const fallback = state.rate || 0.055;
      $('rateValue').textContent = fallback.toFixed(4) + '（可手動覆蓋）';
      $('rateInput').value = fallback.toFixed(4);
      state.rate = fallback; save(); updateBudgetSum();
    }

    // Itinerary
    function renderItinerary(){
      const list = $('it_list');
      list.innerHTML = '';
      if(!state.itinerary.length){
        const div = document.createElement('div');
        div.className = 'empty'; div.textContent = '尚未加入任何行程。';
        list.appendChild(div);
      }else{
        state.itinerary.forEach((it, idx)=>{
          const item = document.createElement('div'); item.className='item';
          const left = document.createElement('div');
          left.innerHTML = `<div>${(it.date||'')+' '+(it.time||'')} - ${it.title||'(未命名)'} @ ${it.place||'—'}</div>
                            <div class="meta">第 ${idx+1} 筆</div>`;
          const right = document.createElement('div');
          const del = document.createElement('button'); del.className='btn-danger'; del.textContent='刪除';
          del.onclick = ()=>{ state.itinerary.splice(idx,1); save(); renderItinerary(); syncMapSelect(); };
          right.appendChild(del);
          item.appendChild(left); item.appendChild(right);
          list.appendChild(item);
        });
      }
      syncMapSelect();
    }
    function addItinerary(){
      const it = {
        title: $('it_title').value.trim(),
        date: $('it_date').value,
        time: $('it_time').value,
        place: $('it_place').value.trim()
      };
      if(!it.title){ alert('請輸入行程項目'); return; }
      state.itinerary.push(it);
      save(); $('it_title').value=''; $('it_date').value=''; $('it_time').value=''; $('it_place').value='';
      renderItinerary();
    }
    function sortItinerary(){
      state.itinerary.sort((a,b)=>{
        const ax = new Date(`${a.date||'2100-01-01'}T${a.time||'00:00'}`).getTime();
        const bx = new Date(`${b.date||'2100-01-01'}T${b.time||'00:00'}`).getTime();
        return ax - bx;
      });
      save(); renderItinerary();
    }
    function clearItinerary(){
      if(confirm('確定刪除所有行程？')){
        state.itinerary = []; save(); renderItinerary(); syncMapSelect();
      }
    }

    // Map
    function syncMapSelect(){
      const sel = $('map_select');
      const prev = sel.value;
      sel.innerHTML = '';
      if(!state.itinerary.length){
        const opt = document.createElement('option');
        opt.value=''; opt.textContent='請先新增行程地點';
        sel.appendChild(opt);
        $('map_iframe').src = '';
        return;
      }
      state.itinerary.forEach(it=>{
        const opt = document.createElement('option');
        opt.value = it.place || '';
        opt.textContent = `${it.title||'(未命名)'} — ${it.place||'—'}`;
        sel.appendChild(opt);
      });
      if(prev){ sel.value = prev; }
      setMap(sel.value);
    }
    function setMap(place){
      if(!place){ $('map_iframe').src=''; return; }
      $('map_iframe').src = `https://www.google.com/maps?q=${encodeURIComponent(place)}&output=embed`;
    }

    // Budget
    function renderBudget(){
      const list = $('bd_list');
      list.innerHTML = '';
      if(!state.budget.length){
        const div = document.createElement('div');
        div.className='empty'; div.textContent='還沒有預算項目。';
        list.appendChild(div);
      }else{
        state.budget.forEach((bd, idx)=>{
          const item = document.createElement('div'); item.className='item';
          const left = document.createElement('div'); left.textContent = `${bd.title||'(未命名)'}：¥${bd.amountJPY||0}`;
          const right = document.createElement('div');
          const del = document.createElement('button'); del.className='btn-danger'; del.textContent='刪除';
          del.onclick = ()=>{ state.budget.splice(idx,1); save(); renderBudget(); updateBudgetSum(); };
          right.appendChild(del);
          item.appendChild(left); item.appendChild(right);
          list.appendChild(item);
        });
      }
      updateBudgetSum();
    }
    function addBudget(){
      const title = $('bd_title').value.trim();
      const amount = Number($('bd_amount').value);
      if(!title){ alert('請輸入項目'); return; }
      if(!Number.isFinite(amount) || amount < 0){ alert('請輸入正確金額'); return; }
      state.budget.push({title, amountJPY: Math.round(amount)});
      save(); $('bd_title').value=''; $('bd_amount').value='';
      renderBudget();
    }
    function clearBudget(){
      if(confirm('確定刪除所有預算項目？')){ state.budget=[]; save(); renderBudget(); updateBudgetSum(); }
    }
    function updateBudgetSum(){
      const sumJPY = state.budget.reduce((s,x)=> s + (x.amountJPY||0), 0);
      $('bd_sum_jpy').textContent = `¥${sumJPY.toLocaleString('en-US')}`;
      const rate = Number($('rateInput').value) || state.rate || 0;
      const sumHKD = sumJPY * rate;
      $('bd_sum_hkd').textContent = `HK$${(sumHKD||0).toFixed(2)}`;
    }

    // Shopping
    function renderShopping(){
      const list = $('sp_list'); const thumbs = $('sp_thumbs');
      list.innerHTML=''; thumbs.innerHTML='';
      if(!state.shopping.length){
        const div = document.createElement('div'); div.className='empty'; div.textContent='尚未加入任何購物品項。';
        list.appendChild(div); return;
      }
      state.shopping.forEach((sp, idx)=>{
        const item = document.createElement('div'); item.className='item';
        const left = document.createElement('div'); left.textContent = `${sp.title||'(未命名)'}${sp.priceJPY?`：¥${sp.priceJPY}`:''}`;
        const right = document.createElement('div');
        const del = document.createElement('button'); del.className='btn-danger'; del.textContent='刪除';
        del.onclick = ()=>{ state.shopping.splice(idx,1); save(); renderShopping(); };
        right.appendChild(del);
        item.appendChild(left); item.appendChild(right);
        list.appendChild(item);

        if(sp.photoDataURL){
          const t = document.createElement('div'); t.className='thumb';
          const img = document.createElement('img'); img.src = sp.photoDataURL; img.alt = sp.title || '購物照片';
          const delBtn = document.createElement('button'); delBtn.className='btn-danger del'; delBtn.textContent='刪除';
          delBtn.onclick = ()=>{ sp.photoDataURL=null; save(); renderShopping(); };
          t.appendChild(img); t.appendChild(delBtn);
          thumbs.appendChild(t);
        }
      });
    }
    function addShopping(){
      const title = $('sp_title').value.trim();
      const price = $('sp_price').value ? Number($('sp_price').value) : null;
      if(!title){ alert('請輸入品項'); return; }
      const file = $('sp_photo').files[0];
      const addItem = (photoDataURL)=>{
        state.shopping.push({title, priceJPY: (price??null), photoDataURL: photoDataURL||null});
        save(); $('sp_title').value=''; $('sp_price').value=''; $('sp_photo').value='';
        renderShopping();
      };
      if(file){
        const reader = new FileReader();
        reader.onload = e => addItem(e.target.result);
        reader.readAsDataURL(file);
      }else{
        addItem(null);
      }
    }
    function clearShopping(){
      if(confirm('確定刪除所有購物品項？')){ state.shopping=[]; save(); renderShopping(); }
    }

    // DOMContentLoaded ensures elements exist before binding
    document.addEventListener('DOMContentLoaded', ()=>{
      // Tabs
      document.querySelectorAll('.tab').forEach(b=>{
        b.addEventListener('click', ()=> show(b.dataset.view));
      });

      // Bind actions
      $('it_add').addEventListener('click', addItinerary);
      $('it_sort').addEventListener('click', sortItinerary);
      $('it_clear').addEventListener('click', clearItinerary);

      $('map_select').addEventListener('change', e=> setMap(e.target.value));
      $('map_open').addEventListener('click', ()=>{
        const place = $('map_select').value;
        if(place){ window.open(`https://www.google.com/maps?q=${encodeURIComponent(place)}`,'_blank'); }
      });

      $('bd_add').addEventListener('click', addBudget);
      $('bd_clear').addEventListener('click', clearBudget);
      $('rateInput').addEventListener('input', ()=>{
        const v = Number($('rateInput').value);
        if(Number.isFinite(v) && v>0){ state.rate = v; save(); $('rateValue').textContent = v.toFixed(4); updateBudgetSum(); }
      });

      $('sp_add').addEventListener('click', addShopping);
      $('sp_clear').addEventListener('click', clearShopping);

      // Init
      load();
      show('itinerary');
      renderItinerary();
      renderBudget();
      renderShopping();
      fetchRateWithRetry();
    });
  </script>
</body>
</html>
