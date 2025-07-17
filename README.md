# Games-you-play
It is a timepass game which is made for people who have suffering from low internet access.
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Coin Runner</title>
  <style>
    body {
      margin: 0;
      font-family: 'Comic Sans MS', cursive, sans-serif;
      background-color: #fff0f5;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    header {
      background-color: #ff69b4;
      width: 100%;
      padding: 1rem 0;
      text-align: center;
      color: white;
      font-size: 2rem;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    }

    .ad-container {
      display: flex;
      justify-content: space-between;
      width: 90%;
      max-width: 1000px;
      margin: 1rem 0;
    }

    .ad-box {
      background-color: #ffe6f0;
      border: 2px dashed #ffb6c1;
      height: 100px;
      width: 23%;
      text-align: center;
      line-height: 100px;
      color: #c71585;
      font-weight: bold;
    }

    canvas {
      border: 3px solid #c71585;
      background-color: #f0ffff;
    }

    #restartButton {
      margin-top: 10px;
      padding: 10px 20px;
      font-size: 16px;
      border: none;
      background-color: #ff69b4;
      color: white;
      border-radius: 5px;
      cursor: pointer;
    }

    footer {
      margin-top: 2rem;
      font-size: 0.9rem;
      color: #555;
    }
  </style>
</head>
<body>
  <header>
    🪙 Coin Runner - Collect Coins & Have Fun!
  </header>

  <div class="ad-container">
    <div class="ad-box">Ad 1</div>
    <div class="ad-box">Ad 2</div>
    <div class="ad-box">Ad 3</div>
    <div class="ad-box">Ad 4</div>
  </div>

  <canvas id="gameCanvas" width="600" height="400"></canvas>
  <button id="restartButton">Restart</button>

  <footer>
    © 2025 Coin Runner by Arsh. All rights reserved.
  </footer>

  <script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");
    const restartButton = document.getElementById("restartButton");

    let player;
    let coins;
    let score;
    let isTouching = false;

    function initGame() {
      player = {
        x: 100,
        y: canvas.height / 2 - 20,
        width: 40,
        height: 40,
        color: "#00f",
      };

      coins = [];
      score = 0;
      for (let i = 0; i < 5; i++) {
        spawnCoin();
      }
    }

    function spawnCoin() {
      const coin = {
        x: canvas.width + Math.random() * 300,
        y: Math.random() * (canvas.height - 30),
        size: 20
      };
      coins.push(coin);
    }

    function drawPlayer() {
      const gradient = ctx.createRadialGradient(
        player.x + player.width / 2,
        player.y + player.height / 2,
        5,
        player.x + player.width / 2,
        player.y + player.height / 2,
        20
      );
      gradient.addColorStop(0, "#00f");
      gradient.addColorStop(1, "#0ff");
      ctx.fillStyle = gradient;
      ctx.fillRect(player.x, player.y, player.width, player.height);
    }

    function drawCoins() {
      ctx.fillStyle = "gold";
      for (const coin of coins) {
        ctx.beginPath();
        ctx.arc(coin.x, coin.y, coin.size / 2, 0, Math.PI * 2);
        ctx.fill();
      }
    }

    function moveCoins() {
      for (const coin of coins) {
        coin.x -= 2;
        if (coin.x < -coin.size) {
          coin.x = canvas.width + Math.random() * 200;
          coin.y = Math.random() * (canvas.height - 30);
        }
      }
    }

    function checkCollection() {
      coins.forEach((coin, i) => {
        if (
          player.x < coin.x + coin.size &&
          player.x + player.width > coin.x &&
          player.y < coin.y + coin.size &&
          player.y + player.height > coin.y
        ) {
          score++;
          coin.x = canvas.width + Math.random() * 200;
          coin.y = Math.random() * (canvas.height - 30);
        }
      });
    }

    function drawScore() {
      ctx.fillStyle = "#333";
      ctx.font = "20px Arial";
      ctx.fillText("Coins: " + score, 10, 30);
    }

    function gameLoop() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      drawPlayer();
      drawCoins();
      moveCoins();
      checkCollection();
      drawScore();

      requestAnimationFrame(gameLoop);
    }

    canvas.addEventListener("touchstart", (e) => {
      isTouching = true;
      movePlayer(e);
    });

    canvas.addEventListener("touchmove", (e) => {
      movePlayer(e);
    });

    canvas.addEventListener("touchend", () => {
      isTouching = false;
    });

    function movePlayer(e) {
      const rect = canvas.getBoundingClientRect();
      const touch = e.touches[0];
      const touchX = touch.clientX - rect.left;
      const touchY = touch.clientY - rect.top;

      player.x = touchX - player.width / 2;
      player.y = touchY - player.height / 2;
    }

    restartButton.addEventListener("click", () => {
      initGame();
    });

    initGame();
    gameLoop();
  </script>
</body>
</html>
