<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>接接樂 - 零食版</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
      background: #fff8e1;
      font-family: sans-serif;
    }
    canvas {
      display: block;
      margin: 0 auto;
      background: #ffe0b2;
    }
  </style>
</head>
<body>
  <canvas id="gameCanvas" width="400" height="600"></canvas>
  <script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');

    const basket = {
      x: 150,
      y: 550,
      width: 100,
      height: 30,
      speed: 5,
    };

    const snacks = [];
    const snackTypes = ['🍫', '🍪', '🍭', '🍿', '🥜'];

    let keys = {};
    let score = 0;

    document.addEventListener('keydown', e => keys[e.key] = true);
    document.addEventListener('keyup', e => keys[e.key] = false);

    function drawBasket() {
      ctx.fillStyle = '#8d6e63';
      ctx.fillRect(basket.x, basket.y, basket.width, basket.height);
    }

    function drawSnack(snack) {
      ctx.font = '24px Arial';
      ctx.fillText(snack.type, snack.x, snack.y);
    }

    function updateBasket() {
      if (keys['ArrowLeft']) basket.x -= basket.speed;
      if (keys['ArrowRight']) basket.x += basket.speed;
      if (keys['ArrowUp']) basket.y -= basket.speed;
      if (keys['ArrowDown']) basket.y += basket.speed;

      // 限制籃子不出邊界
      basket.x = Math.max(0, Math.min(canvas.width - basket.width, basket.x));
      basket.y = Math.max(0, Math.min(canvas.height - basket.height, basket.y));
    }

    function spawnSnack() {
      const type = snackTypes[Math.floor(Math.random() * snackTypes.length)];
      snacks.push({
        x: Math.random() * (canvas.width - 24),
        y: -30,
        type: type,
        speed: 2 + Math.random() * 2,
      });
    }

    function updateSnacks() {
      for (let i = snacks.length - 1; i >= 0; i--) {
        snacks[i].y += snacks[i].speed;
        if (
          snacks[i].y > basket.y &&
          snacks[i].x > basket.x &&
          snacks[i].x < basket.x + basket.width &&
          snacks[i].y < basket.y + basket.height
        ) {
          snacks.splice(i, 1);
          score++;
        } else if (snacks[i].y > canvas.height) {
          snacks.splice(i, 1);
        }
      }
    }

    function drawScore() {
      ctx.fillStyle = '#5d4037';
      ctx.font = '20px Arial';
      ctx.fillText(`得分：${score}`, 10, 30);
    }

    function gameLoop() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      updateBasket();
      updateSnacks();

      drawBasket();
      snacks.forEach(drawSnack);
      drawScore();

      requestAnimationFrame(gameLoop);
    }

    setInterval(spawnSnack, 1000);
    gameLoop();
  </script>
</body>
</html>
