<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>動物園夾娃娃機</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      background: linear-gradient(to bottom, #dfffd6, #ffffff);
      font-family: Arial, sans-serif;
    }
    canvas {
      background: url('https://i.imgur.com/8vU9o5h.jpg') no-repeat center center;
      background-size: cover;
      border: 4px solid #28a745;
      margin-top: 20px;
    }
    button {
      margin: 10px;
      padding: 10px 20px;
      font-size: 18px;
      background-color: #28a745;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    #scoreboard {
      font-size: 20px;
      margin: 10px;
    }
  </style>
</head>
<body>
  <h1>動物園夾娃娃機</h1>
  <div id="scoreboard">分數：0 | 最高分：0 | 時間：20</div>
  <canvas id="gameCanvas" width="400" height="500"></canvas>
  <button onclick="triggerClaw()">夾一下！</button>
  <button onclick="startGame()">重新開始</button>
  <script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");
    const scoreboard = document.getElementById("scoreboard");
    let clawX = 160;
    let clawY = 0;
    let movingLeft = false;
    let movingRight = false;
    let goingDown = false;
    let score = 0;
    let highScore = 0;
    let timeLeft = 20;
    let timer;
    let gameRunning = false;
    let clawPath = [];

    const animals = [];
    const animalImages = [
      'https://i.imgur.com/3YQx0MS.png', // lion
      'https://i.imgur.com/xv34jCB.png', // elephant
      'https://i.imgur.com/O5h92kd.png', // monkey
      'https://i.imgur.com/JZqXDfv.png', // zebra
      'https://i.imgur.com/digZCUq.png'  // giraffe
    ];

    function loadImage(url) {
      const img = new Image();
      img.src = url;
      return img;
    }

    const loadedImages = animalImages.map(loadImage);

    function drawClaw() {
      ctx.fillStyle = 'gray';
      ctx.fillRect(clawX, clawY, 20, 20);
      ctx.strokeStyle = 'gray';
      ctx.beginPath();
      ctx.moveTo(clawX + 10, 0);
      ctx.lineTo(clawX + 10, clawY);
      ctx.stroke();
    }

    function drawPath() {
      if (clawPath.length > 1) {
        ctx.strokeStyle = 'rgba(0,0,0,0.3)';
        ctx.beginPath();
        ctx.moveTo(clawPath[0].x, clawPath[0].y);
        for (let i = 1; i < clawPath.length; i++) {
          ctx.lineTo(clawPath[i].x, clawPath[i].y);
        }
        ctx.stroke();
      }
    }

    function drawAnimals() {
      for (let a of animals) {
        if (!a.caught) {
          ctx.drawImage(a.image, a.x, a.y, 30, 30);
        }
      }
    }

    function spawnAnimals() {
      animals.length = 0;
      for (let i = 0; i < 10; i++) {
        animals.push({
          x: Math.random() * 370,
          y: 300 + Math.random() * 170,
          image: loadedImages[Math.floor(Math.random() * loadedImages.length)],
          caught: false
        });
      }
    }

    function update() {
      if (!gameRunning) return;

      if (movingLeft && clawX > 0) clawX -= 5;
      if (movingRight && clawX < canvas.width - 20) clawX += 5;

      if (goingDown) {
        clawY += 5;
        clawPath.push({x: clawX + 10, y: clawY + 10});
      }

      if (clawY > 400) {
        for (let a of animals) {
          if (!a.caught && clawX < a.x + 30 && clawX + 20 > a.x && clawY < a.y + 30 && clawY + 20 > a.y) {
            a.caught = true;
            score++;
          }
        }
        goingDown = false;
        clawY = 0;
        clawPath = [];
      }

      draw();
    }

    function draw() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      drawPath();
      drawClaw();
      drawAnimals();
      scoreboard.textContent = `分數：${score} | 最高分：${highScore} | 時間：${timeLeft}`;
    }

    function startGame() {
      score = 0;
      timeLeft = 20;
      clawX = 160;
      clawY = 0;
      clawPath = [];
      gameRunning = true;
      spawnAnimals();
      if (timer) clearInterval(timer);
      timer = setInterval(() => {
        timeLeft--;
        if (timeLeft <= 0) {
          clearInterval(timer);
          gameRunning = false;
          if (score > highScore) highScore = score;
        }
      }, 1000);
    }

    function triggerClaw() {
      if (!goingDown && clawY === 0 && gameRunning) {
        goingDown = true;
        clawPath = [{x: clawX + 10, y: clawY + 10}];
      }
    }

    document.addEventListener('keydown', e => {
      if (e.key === 'ArrowLeft') movingLeft = true;
      if (e.key === 'ArrowRight') movingRight = true;
    });

    document.addEventListener('keyup', e => {
      if (e.key === 'ArrowLeft') movingLeft = false;
      if (e.key === 'ArrowRight') movingRight = false;
    });

    function gameLoop() {
      update();
      requestAnimationFrame(gameLoop);
    }
    gameLoop();
  </script>
</body>
</html>
