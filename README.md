<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Мини-Кликер</title>
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <style>
    body {
      margin: 0;
      padding: 20px;
      font-family: sans-serif;
      background: #f0f0f0;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
    }
    #coins {
      font-size: 2em;
      margin-bottom: 20px;
    }
    #clickBtn {
      padding: 15px 30px;
      font-size: 1.2em;
      background: #0088cc;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div id="coins">Монет: 0</div>
  <button id="clickBtn">💰 Клик!</button>

  <script>
    let coins = 0;
    const coinsEl = document.getElementById('coins');
    const btn = document.getElementById('clickBtn');

    // Инициализация Telegram WebApp
    Telegram.WebApp.ready();
    Telegram.WebApp.expand(); // раскрыть на весь экран

    btn.addEventListener('click', () => {
      coins++;
      coinsEl.textContent = `Монет: ${coins}`;
    });
  </script>
</body>
</html>
