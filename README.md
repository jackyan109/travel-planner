<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>旅行行程規劃</title>
  <style>
    :root {
      --bg: #0b0f14;
      --card: #11161d;
      --text: #e7eef7;
      --muted: #9fb2c7;
      --primary: #4db5ff;
      --accent: #74f0a6;
      --danger: #ff6b6b;
      --border: #1f2a36;
      --focus: #7cc5ff;
      --gap: 12px;
      --radius: 12px;
    }
    * { box-sizing: border-box; }
    body {
      margin: 0;
      background: linear-gradient(180deg, #0b0f14 0%, #0e141b 100%);
      color: var(--text);
      font-family: "Inter", "Noto Sans TC", system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial, "PingFang TC", "Microsoft JhengHei", sans-serif;
    }
    header {
      padding: 20px;
      border-bottom: 1px solid var(--border);
      position: sticky;
      top: 0;
      background: rgba(11, 15, 20, 0.85);
      backdrop-filter: blur(8px);
      z-index: 10;
    }
    header h1 {
      margin: 0;
      font-size: 20px;
      letter-spacing: 0.4px;
    }
    header .trip-meta {
      margin-top: 6px;
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 8px;
    }
    header .trip-meta input, header .trip-meta select {
      width: 100%;
      padding: 10px 12px;
      border-radius: 10px;
      border: 1px solid var(--border);
      background: var(--card);
      color: var(--text);
      outline: none;
    }
    header .trip-meta input:focus, header .trip-meta select:focus {
      border-color: var(--focus);
      box-shadow: 0 0 0 3px rgba(77, 181, 255, 0.18);
    }
    main {
      padding: 16px;
      max-width: 960px;
      margin: 0 auto;
    }
    .controls {
      display: grid;
      gap: var(--gap);
      grid-template-columns: 1fr;
      margin-bottom: 12px;
    }
    .card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 14px;
    }
    .grid-2 {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: var(--gap);
    }
    .grid-3 {
      display: grid;
      grid-template-columns: 1.2fr 0.9fr 0.9fr;
      gap: var(--gap);
    }
    label {
      font-size: 12px;
      color: var(--muted);
      margin-bottom: 6px;
      display: block;
    }
    input, textarea, select {
      width: 100%;
      padding: 10px 12px;
      border-radius: 10px;
      border: 1px solid var(--border);
      background: #0f141a;
      color: var(--text);
      outline: none;
    }
    textarea { min-height: 72px; resize: vertical; }
    input:focus, textarea:focus, select:focus {
      border-color: var(--focus);
      box-shadow: 0 0 0 3px rgba(77, 181, 255, 0.16);
    }
    .actions {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
      margin-top: 12px;
    }
    button {
      border: none;
      border-radius: 10px;
      padding: 10px 14px;
      font-weight: 600;
      cursor: pointer;
      transition: transform 0.05s ease, background 0.2s ease, opacity 0.2s ease;
    }
    button:active { transform: scale(0.98); }
    .btn-primary { background: var(--primary); color: #00121c; }
    .btn-secondary { background: #243243; color: var(--text); }
    .btn-danger { background: var(--danger); color: #1a0b0b; }
    .btn-ghost { background: transparent; color: var(--text); border: 1px solid var(--border); }
    .list {
      display: grid;
      gap: var(--gap);
    }
    .item {
      display: grid;
      gap: 10px;
      grid-template-columns: 1fr auto;
      align-items: start;
      border: 1px solid var(--border);
      border-radius: var(--radius);
      background: #0f141a;
      padding: 12px;
    }
    .item h3 {
      margin: 0;
      font-size: 16px;
    }
    .meta {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      color: var(--muted);
      font-size: 13px;
    }
    .badge {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      padding: 6px 10px;
      border-radius: 999px;
      border: 1px solid var(--border);
      background: #0c1218;
      color: var(--text);
      font-size: 12px;
    }
    .empty {
      text-align: center;
      color: var(--muted);
      border: 1px dashed var(--border);
      border-radius: var(--radius);
      padding: 20px;
      background: #0d1319;
    }
    .toolbar {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
      align-items: center;
      justify-content: space-between;
      margin: 12px 0;
    }
    .toolbar .left, .toolbar .right { display: flex; gap: 8px; flex-wrap: wrap; }
    .footer {
      margin-top: 16px;
      color: var(--muted);
      font-size: 12px;
      text-align: center;
    }
    .sr-only {
      position: absolute;
      width: 1px; height: 1px;
      padding: 0; margin: -1px;
      overflow: hidden; clip: rect(0, 0, 0, 0);
      border: 0;
    }
    @media (min-width: 680px) {
      .controls { grid-template-columns: 1fr 1fr; }
    }
  </style>
</head>
<body>
  <header>
    <h1>旅行行程規劃 Travel Planner</h1>
    <div class="trip-meta">
      <input type="text" id="tripName" placeholder="行程名稱（例如：東京五日遊）" />
      <input type="text" id="tripLocation" placeholder="目的地（例如：東京）" />
    </div>
  </header>

  <main>
    <section class="card controls">
      <div class="grid-3">
        <div>
          <label for="title">行程項目</label>
          <input id="title" type="text" placeholder="例如：淺草寺參觀 / 壽司午餐" />
        </div>
        <div>
          <label for="date">日期</label>
          <input id="date" type="date" />
        </div>
        <div>
          <label for="time">時間</label>
          <input id="time" type="time" />
        </div>
      </div>

      <div class="grid-2">
        <div>
          <label for="category">類別</label>
          <select id="category">
            <option value="景點">景點</option>
            <option value="美食">美食</option>
            <option value="交通">交通</option>
            <option value="住宿">住宿</option>
            <option value="其他">其他</option>
          </select>
        </div>
        <div>
          <label for="cost">預估花費（HKD）</label>
          <input id="cost" type="number" min="0" step="1" placeholder="例如：120" />
        </div>
      </div>

      <div>
        <label for="notes">備註</label>
        <textarea id="notes" placeholder="例如：需預約、購票連結、集合地點…"></textarea>
      </div>

      <div class="actions">
        <button class="btn-primary" id="addBtn">新增項目</button>
        <button class="btn-secondary" id="clearInputsBtn">清空輸入</button>
      </div>
    </section>

    <div class="toolbar">
      <div class="left">
        <button class="btn-ghost" id="sortBtn" aria-label="按日期與時間排序">排序</button>
        <button class="btn-ghost" id="filterTodayBtn">只看今日</button>
        <button class="btn-ghost" id="showAllBtn">顯示全部</button>
      </div>
      <div class="right">
        <button class="btn-secondary" id="exportBtn">匯出 JSON</button>
        <button class="btn-secondary" id="importBtn">匯入 JSON</button>
        <input type="file" id="fileInput" accept="application/json" class="sr-only" />
        <button class="btn-danger" id="clearAllBtn">清除全部</button>
      </div>
    </div>

    <section id="list" class="list">
      <div class="empty">目前沒有行程項目。新增你的第一個安排吧！</div>
    </section>

    <div class="footer">資料儲存在你的瀏覽器（localStorage）。適合手機瀏覽與操作。</div>
  </main>

  <script>
    const els = {
      tripName: document.getElementById('tripName'),
      tripLocation: document.getElementById('tripLocation'),
      title: document.getElementById('title'),
      date: document.getElementById('date'),
      time: document.getElementById('time'),
      category: document.getElementById('category'),
      cost: document.getElementById('cost'),
      notes: document.getElementById('notes'),
      addBtn: document.getElementById('addBtn'),
      clearInputsBtn: document.getElementById('clearInputsBtn'),
      sortBtn: document.getElementById('sortBtn'),
      filterTodayBtn: document.getElementById('filterTodayBtn'),
      showAllBtn: document.getElementById('showAllBtn'),
      exportBtn: document.getElementById('exportBtn'),
      importBtn: document.getElementById('importBtn'),
      fileInput: document.getElementById('fileInput'),
      clearAllBtn: document.getElementById('clearAllBtn'),
      list: document.getElementById('list')
    };

    const STORAGE_KEY = 'travel_planner_v1';
    let items = [];

    function load() {
      const raw = localStorage.getItem(STORAGE_KEY);
      if (raw) {
        try {
          const data = JSON.parse(raw);
          items = Array.isArray(data.items) ? data.items : [];
          els.tripName.value = data.tripName || '';
          els.tripLocation.value = data.tripLocation || '';
        } catch {}
      }
      render();
    }

    function save() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify({
        tripName: els.tripName.value.trim(),
        tripLocation: els.tripLocation.value.trim(),
        items
      }));
    }

    function render(list = items) {
      els.list.innerHTML = '';
      if (!list.length) {
        const div = document.createElement('div');
        div.className = 'empty';
        div.textContent = '目前沒有行程項目。新增你的第一個安排吧！';
        els.list.appendChild(div);
        return;
      }
      list.forEach((it, idx) => {
        const wrap = document.createElement('div');
        wrap.className = 'item';

        const left = document.createElement('div');
        const title = document.createElement('h3');
        title.textContent = it.title || '(未命名項目)';
        left.appendChild(title);

        const meta = document.createElement('div');
        meta.className = 'meta';

        const timeBadge = document.createElement('span');
        timeBadge.className = 'badge';
        timeBadge.textContent = `${it.date || '未設定'} ${it.time || ''}`.trim();
        meta.appendChild(timeBadge);

        const catBadge = document.createElement('span');
        catBadge.className = 'badge';
        catBadge.textContent = `類別：${it.category || '—'}`;
        meta.appendChild(catBadge);

        if (it.cost !== '' && it.cost !== null && it.cost !== undefined) {
          const costBadge = document.createElement('span');
          costBadge.className = 'badge';
          costBadge.textContent = `HKD ${it.cost}`;
          meta.appendChild(costBadge);
        }

        left.appendChild(meta);

        if (it.notes && it.notes.trim()) {
          const notes = document.createElement('div');
          notes.style.color = '#c8d7e6';
          notes.style.fontSize = '13px';
          notes.style.marginTop = '6px';
          notes.textContent = it.notes.trim();
          left.appendChild(notes);
        }

        const right = document.createElement('div');
        right.style.display = 'flex';
        right.style.flexDirection = 'column';
        right.style.gap = '8px';

        const editBtn = document.createElement('button');
        editBtn.className = 'btn-secondary';
        editBtn.textContent = '編輯';
        editBtn.addEventListener('click', () => editItem(idx));
        right.appendChild(editBtn);

        const delBtn = document.createElement('button');
        delBtn.className = 'btn-danger';
        delBtn.textContent = '刪除';
        delBtn.addEventListener('click', () => {
          items.splice(idx, 1);
          save(); render();
        });
        right.appendChild(delBtn);

        wrap.appendChild(left);
        wrap.appendChild(right);
        els.list.appendChild(wrap);
      });
    }

    function addItem() {
      const item = {
        id: crypto.randomUUID ? crypto.randomUUID() : String(Date.now() + Math.random()),
        title: els.title.value.trim(),
        date: els.date.value,
        time: els.time.value,
        category: els.category.value,
        cost: els.cost.value ? Number(els.cost.value) : '',
        notes: els.notes.value.trim()
      };
      if (!item.title) {
        alert('請輸入行程項目名稱');
        return;
      }
      items.push(item);
      save();
      clearInputs();
      render();
    }

    function clearInputs() {
      els.title.value = '';
      els.date.value = '';
      els.time.value = '';
      els.category.value = '景點';
      els.cost.value = '';
      els.notes.value = '';
    }

    function editItem(index) {
      const it = items[index];
      els.title.value = it.title || '';
      els.date.value = it.date || '';
      els.time.value = it.time || '';
      els.category.value = it.category || '其他';
      els.cost.value = it.cost !== '' ? it.cost : '';
      els.notes.value = it.notes || '';

      // Replace "新增項目" with "更新項目" temporarily
      els.addBtn.textContent = '更新項目';
      els.addBtn.classList.remove('btn-primary');
      els.addBtn.classList.add('btn-secondary');

      const handler = () => {
        it.title = els.title.value.trim();
        it.date = els.date.value;
        it.time = els.time.value;
        it.category = els.category.value;
        it.cost = els.cost.value ? Number(els.cost.value) : '';
        it.notes = els.notes.value.trim();
        save(); render(); clearInputs();
        els.addBtn.textContent = '新增項目';
        els.addBtn.classList.remove('btn-secondary');
        els.addBtn.classList.add('btn-primary');
        els.addBtn.removeEventListener('click', handler);
        els.addBtn.addEventListener('click', addItem);
      };

      els.addBtn.removeEventListener('click', addItem);
      els.addBtn.addEventListener('click', handler);
    }

    function sortByDateTime() {
      items.sort((a, b) => {
        const ax = new Date(`${a.date || '2100-01-01'}T${a.time || '00:00'}`).getTime();
        const bx = new Date(`${b.date || '2100-01-01'}T${b.time || '00:00'}`).getTime();
        return ax - bx;
      });
      save(); render();<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>我的旅行行程</title>
  <style>
    body {
      font-family: "Noto Sans TC", Arial, sans-serif;
      background: #f0f4f8;
      margin: 0;
      padding: 20px;
    }
    h1 {
      color: #2c3e50;
    }
    .trip {
      background: #fff;
      border-radius: 8px;
      padding: 15px;
      margin-bottom: 10px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    .trip h2 {
      margin: 0 0 5px;
      color: #2980b9;
    }

</body>
</html>
