<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Car Drift Game</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            font-family: Arial, sans-serif;
        }
        canvas {
            display: block;
        }
        #score {
            position: absolute;
            top: 10px;
            left: 10px;
            color: white;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <div id="score">Score: 0</div>
    <canvas id="gameCanvas"></canvas>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        // Game variables
        let score = 0;
        const car = {
            x: canvas.width / 2,
            y: canvas.height - 100,
            width: 50,
            height: 100,
            angle: 0,
            speed: 0,
            maxSpeed: 5,
            friction: 0.98,
            turnSpeed: 0.05,
        };

        const keys = {
            ArrowUp: false,
            ArrowDown: false,
            ArrowLeft: false,
            ArrowRight: false,
        };

        // Handle key events
        window.addEventListener("keydown", (e) => {
            if (keys.hasOwnProperty(e.key)) keys[e.key] = true;
        });

        window.addEventListener("keyup", (e) => {
            if (keys.hasOwnProperty(e.key)) keys[e.key] = false;
        });

        function drawCar() {
            ctx.save();
            ctx.translate(car.x, car.y);
            ctx.rotate(car.angle);
            ctx.fillStyle = "red";
            ctx.fillRect(-car.width / 2, -car.height / 2, car.width, car.height);
            ctx.restore();
        }

        function updateCar() {
            if (keys.ArrowUp) {
                car.speed = Math.min(car.speed + 0.1, car.maxSpeed);
            }

            if (keys.ArrowDown) {
                car.speed = Math.max(car.speed - 0.1, -car.maxSpeed / 2);
            }

            if (keys.ArrowLeft) {
                car.angle -= car.turnSpeed * (car.speed / car.maxSpeed);
            }

            if (keys.ArrowRight) {
                car.angle += car.turnSpeed * (car.speed / car.maxSpeed);
            }

            car.speed *= car.friction;
            car.x += Math.sin(car.angle) * car.speed;
            car.y -= Math.cos(car.angle) * car.speed;

            if (car.x < 0 || car.x > canvas.width || car.y < 0 || car.y > canvas.height) {
                car.x = canvas.width / 2;
                car.y = canvas.height - 100;
                car.speed = 0;
                score = 0;
            }
        }

        function updateScore() {
            score += Math.abs(car.speed);
            document.getElementById("score").textContent = `Score: ${Math.floor(score)}`;
        }

        function gameLoop() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawCar();
            updateCar();
            updateScore();
            requestAnimationFrame(gameLoop);
        }

        gameLoop();
    </script>
</body>
</html>
# car-drift-2
