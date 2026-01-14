<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Мини-Кликер</title>
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      font-family: 'Segoe UI', system-ui, sans-serif;
      color: white;
      overflow-x: hidden;
      padding: 20px;
      touch-action: manipulation; /* Отключает двойной тап и масштаб */
    }

    .container {
      text-align: center;
      max-width: 500px;
    }

    .coins-display {
      font-size: 3rem;
      font-weight: bold;
      margin-bottom: 30px;
      text-shadow: 0 2px 6px rgba(0,0,0,0.3);
      letter-spacing: 1px;
    }

    .coin-button {
      width: 160px;
      height: 160px;
      background: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxMDAiIGhlaWdodD0iMTAwIiB2aWV3Qm94PSIwIDAgMTAwIDEwMCI+PGNpcmNsZSBjeD0iNTAiIGN5PSI1MCIgcj0iNDUiIGZpbGw9IiNmZmYiIHN0cm9rZT0iIzFFODhEQyIgc3Ryb2tlLXdpZHRoPSIzIi8+PGNpcmNsZSBjeD0iNTAiIGN5PSI1MCIgcj0iMzAiIGZpbGw9IiMxRTg4REMifC8+PHRleHQgeD0iNTAiIHk9IjUwIiBmb250LWZhbWlseT0iQm9sZCIgZm9udC1zaXplPSIyMCIgZmlsbD0iI2ZmZiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZG9taW5hbnQtYmFzZWxpbmU9Im1pZGRsZSI+JiMxMjc4OTY7PC90ZXh0Pjwvc3ZnPg==') no-repeat center;
      background-size: contain;
      border: none;
      cursor: pointer;
      position: relative;
      transition: transform 0.2s ease;
      box-shadow: 0 10px 30px rgba(0,0,0,0.3);
      border-radius: 50%;
      outline: none;
      -webkit-tap-highlight-color: transparent; /* Убирает подсветку при клике */
    }

    .coin-button:active {
      transform: scale(0.92);
    }

    .coin-button:hover {
      transform: scale(1.05);
    }

    .plus-one {
      position: absolute;
      font-size: 1.6rem;
      font-weight: bold;
      color: #fff;
      opacity: 0;
      animation: floatUp 1s forwards;
      pointer-events: none;
      z-index: 10;
      text-shadow: 0 1px 2px rgba(0,0,0,0.5);
    }

    @keyframes floatUp {
      0% {
        opacity: 1;
        transform: translateY(0) scale(1);
      }
      100% {
        opacity: 0;
        transform: translateY(-60px) scale(1.2);
      }
    }

    /* Мобильная адаптация */
    @media (max-width: 600px) {
      .coins-display {
        font-size: 2.4rem;
      }
      .coin-button {
        width: 130px;
        height: 130px;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="coins-display">Монет: <span id="coins">0</span></div>
    <button class="coin-button" id="coinBtn"></button>
  </div>

  <script>
    let coins = 0;
    const coinsEl = document.getElementById('coins');
    const btn = document.getElementById('coinBtn');

    // Инициализация Telegram WebApp
    if (window.Telegram && Telegram.WebApp) {
      Telegram.WebApp.ready();
      Telegram.WebApp.expand();
    }

    btn.addEventListener('click', (e) => {
      e.preventDefault(); // Отменяем дефолтное поведение
      coins++;
      coinsEl.textContent = coins;

      // Создаём анимацию "+1"
      const plusOne = document.createElement('div');
      plusOne.className = 'plus-one';
      plusOne.textContent = '+1';
      
      // Позиция над кнопкой
      const rect = btn.getBoundingClientRect();
      plusOne.style.left = `${rect.left + rect.width / 2}px`;
      plusOne.style.top = `${rect.top + rect.height / 2}px`;

      document.body.appendChild(plusOne);

      // Удаляем после анимации
      setTimeout(() => {
        if (plusOne.parentNode) {
          plusOne.parentNode.removeChild(plusOne);
        }
      }, 1000);
    });
  </script>
</body>
</html>
