<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <title>行程表測試</title>
  <style>
    body { font-family: "Noto Sans TC", Arial, sans-serif; background:#f0f4f8; margin:20px; }
    input { margin:5px; padding:6px; }
    button { margin:5px; padding:6px 10px; cursor:pointer; }
    .item {
      background:#fff; border:1px solid #ccc; border-radius:6px;
      padding:8px; margin:6px 0; display:flex; justify-content:space-between; align-items:center;
    }
  </style>
</head>
<body>
  <h2>行程表</h2>
  <input id="it_title" placeholder="行程項目">
  <input id="it_date" type="date">
  <input id="it_time" type="time">
  <input id="it_place" placeholder="地點">
  <button id="it_add">新增行程</button>

  <div id="it_list"></div>

  <script>
    const itinerary = [];

    // 綁定新增按鍵
    document.getElementById('it_add').addEventListener('click', addItinerary);

    function addItinerary(){
      const title = document.getElementById('it_title').value;
      const date = document.getElementById('it_date').value;
      const time = document.getElementById('it_time').value;
      const place = document.getElementById('it_place').value;

      if(!title){ alert("請輸入行程項目"); return; }

      itinerary.push({title,date,time,place});
      renderItinerary();

      // 清空輸入框
      document.getElementById('it_title').value='';
      document.getElementById('it_date').value='';
      document.getElementById('it_time').value='';
      document.getElementById('it_place').value='';
    }

    function renderItinerary(){
      const list = document.getElementById('it_list');
      list.innerHTML = '';
      itinerary.forEach((it, index)=>{
        const div = document.createElement('div');
        div.className = 'item';
        div.innerHTML = `
          <span>${it.date} ${it.time} - ${it.title} @ ${it.place}</span>
          <button onclick="deleteItinerary(${index})">刪除</button>
        `;
        list.appendChild(div);
      });
    }

    function deleteItinerary(index){
      itinerary.splice(index,1); // 移除指定行程
      renderItinerary();         // 重新渲染列表
    }
  </script>
</body>
</html>
