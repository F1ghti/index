<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Кликер Монет</title>
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
      touch-action: manipulation;
      animation: gradientShift 15s ease infinite; /* Анимация фона */
    }

    @keyframes gradientShift {
      0% { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); }
      50% { background: linear-gradient(135deg, #764ba2 0%, #667eea 100%); }
      100% { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); }
    }

    .container {
      text-align: center;
      max-width: 90vw;
      width: 100%;
    }

    .coins-display {
      font-size: 3rem;
      font-weight: bold;
      margin-bottom: 30px;
      text-shadow: 0 2px 6px rgba(0,0,0,0.3);
      letter-spacing: 1px;
    }

    .coin-button {
      width: 140px;
      height: 140px;
      background: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxMDAiIGhlaWdodD0iMTAwIiB2aWV3Qm94PSIwIDAgMTAwIDEwMCI+PGNpcmNsZSBjeD0iNTAiIGN5PSI1MCIgcj0iNDUiIGZpbGw9IiNmZmYiIHN0cm9rZT0iIzFFODhEQyIgc3Ryb2tlLXdpZHRoPSIzIi8+PGNpcmNsZSBjeD0iNTAiIGN5PSI1MCIgcj0iMzAiIGZpbGw9IiMxRTg4REMifC8+PHRleHQgeD0iNTAiIHk9IjUwIiBmb250LWZhbWlseT0iQm9sZCIgZm9udC1zaXplPSIyMCIgZmlsbD0iI2ZmZiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZG9taW5hbnQtYmFzZWxpbmU9Im1pZGRsZSI+JiMxMjc4OTY7PC90ZXh0Pjwvc3ZnPg==') no-repeat center;
      background-size: contain;
      border: none;
      cursor: pointer;
      position: relative;
      transition: transform 0.2s ease, box-shadow 0.2s ease;
      box-shadow: 0 10px 30px rgba(0,0,0,0.3);
      border-radius: 50%;
      outline: none;
      -webkit-tap-highlight-color: transparent;
    }

    .coin-button:active {
      transform: scale(0.92);
      box-shadow: 0 5px 15px rgba(0,0,0,0.4);
    }

    .coin-button:hover {
      transform: scale(1.05);
      box-shadow: 0 15px 40px rgba(0,0,0,0.4);
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

    /* Падающие монетки */
    .falling-coin {
      position: absolute;
      top: -50px;
      left: 50%;
      transform: translateX(-50%);
      width: 40px;
      height: 40px;
      background: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0MCIgaGVpZ2h0PSI0MCIgdmlld0JveD0iMCAwIDQwIDQwIj48Y2lyY2xlIGN4PSIyMCIgY3k9IjIwIiByPSIxOCIgZmlsbD0iI2ZmZiIgc3Ryb2tlPSIjMUU4OERDIiBzdHJva2Utd2lkdGg9IjIiLz48Y2lyY2xlIGN4PSIyMCIgY3k9IjIwIiByPSIxMiIgZmlsbD0iIzFFODhEQyIvPjx0ZXh0IHg9IjIwIiB5PSIyMCIgZm9udC1mYW1pbHk9IkJvbGQiIGZvbnQtc2l6ZT0iMTIiIGZpbGw9IiNmZmYiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGRvbWluYW50LWJhc2VsaW5lPSJtaWRkbGUiPiYjMTI3ODk2OzwvdGV4dD48L3N2Zz4=') no-repeat center;
      background-size: contain;
      animation: fall 3s linear forwards;
      pointer-events: none;
      z-index: -1;
    }

    @keyframes fall {
      0% { transform: translateX(-50%) translateY(-50px); opacity: 0; }
      10% { opacity: 1; }
      90% { opacity: 1; }
      100% { transform: translateX(-50%) translateY(100vh); opacity: 0; }
    }

    /* Адаптивность */
    @media (max-width: 600px) {
      .coins-display {
        font-size: 2.2rem;
      }
      .coin-button {
        width: 110px;
        height: 110px;
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
      e.preventDefault();
      coins++;
      coinsEl.textContent = coins;

      // Создаём анимацию "+1"
      const plusOne = document.createElement('div');
      plusOne.className = 'plus-one';
      plusOne.textContent = '+1';
      
      const rect = btn.getBoundingClientRect();
      plusOne.style.left = `${rect.left + rect.width / 2}px`;
      plusOne.style.top = `${rect.top + rect.height / 2}px`;

      document.body.appendChild(plusOne);

      setTimeout(() => {
        if (plusOne.parentNode) {
          plusOne.parentNode.removeChild(plusOne);
        }
      }, 1000);

      // Создаём падающую монетку
      const fallingCoin = document.createElement('div');
      fallingCoin.className = 'falling-coin';
      fallingCoin.style.left = Math.random() * 100 + '%'; // случайное положение по горизонтали
      document.body.appendChild(fallingCoin);

      setTimeout(() => {
        if (fallingCoin.parentNode) {
          fallingCoin.parentNode.removeChild(fallingCoin);
        }
      }, 3000);

      // Опционально: звук
      // new Audio('data:audio/wav;base64,UklGRigAAABXQVZFZm10IBAAAAABAAEAgD4AAAB9AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA').play();
    });
  </script>
</body>
</html>
