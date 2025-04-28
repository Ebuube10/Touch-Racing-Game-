# Touch-Racing-Game-
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Touch Racing Game</title>
  <style>
    body {
      text-align: center;
      font-family: sans-serif;
      background: #111;
      color: white;
    }
    canvas {
      background: #333;
      display: block;
      margin: 0 auto;
      border: 2px solid #000;
    }
    .controls {
      margin-top: 10px;
    }
    .btn {
      padding: 15px 25px;
      margin: 5px;
      font-size: 18px;
      background: #008080;
      color: white;
      border: none;
      border-radius: 8px;
    }
  </style>
</head>
<body>
  <h2>Touch Racing Game</h2>
  <canvas id="game" width="300" height="500"></canvas>

  <div class="controls">
    <button class="btn" onclick="moveLeft()">Left</button>
    <button class="btn" onclick="moveRight()">Right</button>
  </div>

  <script>
    const canvas = document.getElementById("game");
    const ctx = canvas.getContext("2d");

    let score = 0;
    let gameOver = false;

    const car = {
      x: 130,
      y: 420,
      width: 40,
      height: 70,
      color: "red",
      speed: 20
    };

    const enemies = [];

    function spawnEnemy() {
      const x = Math.floor(Math.random() * (canvas.width - 40));
      enemies.push({
        x: x,
        y: -70,
        width: 40,
        height: 70,
        speed: 4,
        color: "blue"
      });
    }

    function drawCar(obj) {
      ctx.fillStyle = obj.color;
      ctx.fillRect(obj.x, obj.y, obj.width, obj.height);
    }

    function detectCollision(rect1, rect2) {
      return (
        rect1.x < rect2.x + rect2.width &&
        rect1.x + rect1.width > rect2.x &&
        rect1.y < rect2.y + rect2.height &&
        rect1.y + rect1.height > rect2.y
      );
    }

    function update() {
      if (gameOver) return;

      ctx.clearRect(0, 0, canvas.width, canvas.height);

      drawCar(car);

      for (let i = enemies.length - 1; i >= 0; i--) {
        let enemy = enemies[i];
        enemy.y += enemy.speed;
        drawCar(enemy);

        if (detectCollision(car, enemy)) {
          gameOver = true;
          alert("Game Over! Score: " + score);
          window.location.reload();
        }

        if (enemy.y > canvas.height) {
          enemies.splice(i, 1);
          score++;
        }
      }

      ctx.fillStyle = "white";
      ctx.font = "16px Arial";
      ctx.fillText("Score: " + score, 10, 20);

      requestAnimationFrame(update);
    }

    function moveLeft() {
      if (car.x > 0) car.x -= car.speed;
    }

    function moveRight() {
      if (car.x + car.width < canvas.width) car.x += car.speed;
    }

    setInterval(spawnEnemy, 1000);
    update();
  </script>
</body>
</html>
