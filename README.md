<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <title>百分比誤差計算機</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 20px;
      background-color: #f9f9f9;
      /* 移除背景圖片 */
    }

    .container {
      max-width: 500px;
      margin: auto;
      background: rgba(255, 255, 255, 0.95);
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.05);
    }

    h2 {
      text-align: center;
      font-size: 24px;
      margin-bottom: 20px;
    }

    label {
      margin-top: 10px;
      display: block;
      font-size: 16px;
    }

    input, button {
      width: 100%;
      padding: 12px;
      margin-top: 5px;
      font-size: 16px;
      box-sizing: border-box;
    }

    button {
      background-color: #0070ba;
      color: white;
      border: none;
      border-radius: 5px;
      margin-top: 15px;
    }

    #result {
      margin-top: 20px;
      font-size: 18px;
      color: #333;
      text-align: center;
    }

    .history-item {
      border-top: 1px dashed #ccc;
      padding-top: 8px;
      margin-top: 8px;
      font-size: 15px;
    }

    h3 {
      margin-top: 30px;
      font-size: 18px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>百分比誤差計算機</h2>

    <label>實際吐出量 (g/min)：</label>
    <input type="number" id="actual" placeholder="輸入實際值">

    <label>設計吐出量 (g/min)：</label>
    <input type="number" id="design" placeholder="輸入設計值">

    <button onclick="calculate()">計算</button>

    <div id="result"></div>

    <h3>歷史紀錄（最多 5 筆）</h3>
    <div id="history"></div>
  </div>

  <script>
    const historyList = [];

    function calculate() {
      const actual = parseFloat(document.getElementById('actual').value);
      const design = parseFloat(document.getElementById('design').value);
      const resultDiv = document.getElementById('result');
      const historyDiv = document.getElementById('history');

      if (isNaN(actual) || isNaN(design) || design === 0) {
        resultDiv.innerHTML = '⚠️ 請輸入有效數值，且設計值不能為 0。';
        return;
      }

      const percentError = ((actual - design) / design) * 100;

      resultDiv.innerHTML = `
        <div>百分比誤差：<strong>${percentError.toFixed(2)}%</strong></div>
      `;

      historyList.unshift({
        actual,
        design,
        percentError: percentError.toFixed(2)
      });
      if (historyList.length > 5) historyList.pop();

      historyDiv.innerHTML = '';
      historyList.forEach((item, idx) => {
        historyDiv.innerHTML += `
          <div class="history-item">
            #${idx + 1} 實際：${item.actual}，設計：${item.design}，
            百分比誤差：${item.percentError}%
          </div>
        `;
      });
    }
  </script>
</body>
</html>
