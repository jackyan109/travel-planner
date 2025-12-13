<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <title>行程表測試</title>
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

    // 綁定按鍵事件
    document.getElementById('it_add').addEventListener('click', addItinerary);

    function addItinerary(){
      const title = document.getElementById('it_title').value;
      const date = document.getElementById('it_date').value;
      const time = document.getElementById('it_time').value;
      const place = document.getElementById('it_place').value;

      if(!title){ alert("請輸入行程項目"); return; }

      itinerary.push({title,date,time,place});
      renderItinerary();
    }

    function renderItinerary(){
      const list = document.getElementById('it_list');
      list.innerHTML = '';
      itinerary.forEach(it=>{
        const div = document.createElement('div');
        div.textContent = `${it.date} ${it.time} - ${it.title} @ ${it.place}`;
        list.appendChild(div);
      });
    }
  </script>
</body>
</html>
