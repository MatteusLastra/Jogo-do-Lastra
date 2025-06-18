# Jogo-de-Carro
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Jogo de Carro Tropical</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
      background: linear-gradient(#87CEEB, #f7dcae); /* céu e areia */
      font-family: sans-serif;
    }

    canvas {
      display: block;
      margin: auto;
      background: #228B22; /* fundo tropical */
    }
  </style>
</head>
<body>
<canvas id="gameCanvas" width="400" height="600"></canvas>

<script>
  const canvas = document.getElementById('gameCanvas');
  const ctx = canvas.getContext('2d');

  const road = {
    x: 100,
    y: 0,
    width: 200,
    height: 600,
    color: '#444'
  };

  const car = {
    x: 190,
    y: 500,
    width: 20,
    height: 40,
    color: 'red',
    speed: 5
  };

  let obstacles = [];
  let keys = {};

  function drawRoad() {
    ctx.fillStyle = road.color;
    ctx.fillRect(road.x, road.y, road.width, road.height);

    // faixas da estrada
    ctx.strokeStyle = 'white';
    ctx.lineWidth = 2;
    for (let i = 0; i < 600; i += 40) {
      ctx.beginPath();
      ctx.moveTo(road.x + road.width / 2, i);
      ctx.lineTo(road.x + road.width / 2, i + 20);
      ctx.stroke();
    }
  }

  function drawCar() {
    ctx.fillStyle = car.color;
    ctx.fillRect(car.x, car.y, car.width, car.height);
  }

  function drawObstacles() {
    ctx.fillStyle = 'black';
    obstacles.forEach((obs) => {
      ctx.fillRect(obs.x, obs.y, obs.width, obs.height);
    });
  }

  function drawTropicalTrees() {
    for (let i = 0; i < 600; i += 80) {
      // esquerda
      ctx.fillStyle = '#8B4513';
      ctx.fillRect(road.x - 20, i + 10, 8, 20);
      ctx.fillStyle = 'green';
      ctx.beginPath();
      ctx.arc(road.x - 16, i + 5, 15, 0, Math.PI * 2);
      ctx.fill();

      // direita
      ctx.fillStyle = '#8B4513';
      ctx.fillRect(road.x + road.width + 12, i + 10, 8, 20);
      ctx.fillStyle = 'green';
      ctx.beginPath();
      ctx.arc(road.x + road.width + 16, i + 5, 15, 0, Math.PI * 2);
      ctx.fill();
    }
  }

  function moveCar() {
    if (keys['ArrowLeft'] && car.x > road.x) {
      car.x -= car.speed;
    }
    if (keys['ArrowRight'] && car.x + car.width < road.x + road.width) {
      car.x += car.speed;
    }
  }

  function updateObstacles() {
    obstacles.forEach((obs) => obs.y += 4);
    if (Math.random() < 0.02) {
      let obsX = road.x + Math.random() * (road.width - 30);
      obstacles.push({ x: obsX, y: -20, width: 30, height: 30 });
    }
    obstacles = obstacles.filter((obs) => obs.y < canvas.height);
  }

  function checkCollision() {
    for (let obs of obstacles) {
      if (
        car.x < obs.x + obs.width &&
        car.x + car.width > obs.x &&
        car.y < obs.y + obs.height &&
        car.y + car.height > obs.y
      ) {
        alert('💥 Você bateu! Fim de jogo.');
        document.location.reload();
      }
    }
  }

  function gameLoop() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    drawRoad();
    drawTropicalTrees();
    moveCar();
    drawCar();
    updateObstacles();
    drawObstacles();
    checkCollision();
    requestAnimationFrame(gameLoop);
  }

  window.addEventListener('keydown', (e) => {
    keys[e.key] = true;
  });
  window.addEventListener('keyup', (e) => {
    keys[e.key] = false;
  });

  gameLoop();
</script>
</body>
</html>
