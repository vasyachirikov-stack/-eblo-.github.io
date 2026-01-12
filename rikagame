<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">
    <title>RikaNV: Лесной полет</title>
    <style>
        body { margin: 0; padding: 0; overflow: hidden; background: #0b0d0f; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; display: flex; justify-content: center; align-items: center; height: 100vh; }
        #game-container { position: relative; width: 100%; max-width: 400px; height: 100vh; max-height: 700px; box-shadow: 0 0 50px rgba(0,0,0,0.5); border: 2px solid #2c3e50; }
        canvas { display: block; background: #1a1c1f; width: 100%; height: 100%; }
        #ui { position: absolute; top: 20px; left: 0; width: 100%; text-align: center; color: white; pointer-events: none; text-transform: uppercase; letter-spacing: 2px; text-shadow: 2px 2px 4px rgba(0,0,0,0.8); }
        #score { font-size: 48px; font-weight: bold; }
        #msg { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); color: white; text-align: center; width: 100%; background: rgba(0,0,0,0.7); padding: 20px 0; display: none; }
        .btn { background: #e67e22; border: none; padding: 10px 20px; color: white; font-weight: bold; cursor: pointer; margin-top: 10px; border-radius: 5px; }
    </style>
</head>
<body>
    <div id="game-container">
        <canvas id="canvas"></canvas>
        <div id="ui">
            <div id="score">0</div>
        </div>
        <div id="msg">
            <h2 id="msg-text">КОНЕЦ ИГРЫ</h2>
            <p>Ваш результат: <span id="final-score">0</span></p>
            <button class="btn" onclick="resetGame()">ИГРАТЬ СНОВА</button>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        const scoreEl = document.getElementById('score');
        const msgEl = document.getElementById('msg');
        const finalScoreEl = document.getElementById('final-score');

        // Настройки под мобильные экраны
        canvas.width = 400;
        canvas.height = 700;

        let bird = { x: 50, y: 300, w: 40, h: 30, v: 0, g: 0.25, jump: -6 };
        let pipes = [];
        let score = 0;
        let active = true;
        let frame = 0;

        function drawBird() {
            ctx.fillStyle = "#e67e22"; // Цвет бренда (оранжевый RikaNV)
            ctx.beginPath();
            ctx.roundRect(bird.x, bird.y, bird.w, bird.h, 5);
            ctx.fill();
            // Глаз/Линза
            ctx.fillStyle = "#34495e";
            ctx.fillRect(bird.x + bird.w - 15, bird.y + 5, 10, 10);
        }

        function createPipe() {
            const gap = 160;
            const minH = 100;
            const h = Math.random() * (canvas.height - gap - minH * 2) + minH;
            pipes.push({ x: canvas.width, top: h, bottom: canvas.height - h - gap, w: 60 });
        }

        function drawPipes() {
            ctx.fillStyle = "#2c3e50";
            pipes.forEach(p => {
                ctx.fillRect(p.x, 0, p.w, p.top);
                ctx.fillRect(p.x, canvas.height - p.bottom, p.w, p.bottom);
            });
        }

        function loop() {
            if (!active) return;
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Фон (имитация сетки)
            ctx.strokeStyle = "rgba(255,255,255,0.05)";
            for(let i=0; i<canvas.width; i+=40) { ctx.beginPath(); ctx.moveTo(i,0); ctx.lineTo(i,canvas.height); ctx.stroke(); }
            for(let i=0; i<canvas.height; i+=40) { ctx.beginPath(); ctx.moveTo(0,i); ctx.lineTo(canvas.width,i); ctx.stroke(); }

            bird.v += bird.g;
            bird.y += bird.v;

            if (frame % 100 === 0) createPipe();

            pipes.forEach((p, i) => {
                p.x -= 3;
                if (p.x + p.w < 0) { pipes.splice(i, 1); score++; scoreEl.innerText = score; }
                
                // Коллизии
                if (bird.x + bird.w > p.x && bird.x < p.x + p.w) {
                    if (bird.y < p.top || bird.y + bird.h > canvas.height - p.bottom) gameOver();
                }
            });

            if (bird.y + bird.h > canvas.height || bird.y < 0) gameOver();

            drawPipes();
            drawBird();
            frame++;
            requestAnimationFrame(loop);
        }

        function gameOver() {
            active = false;
            msgEl.style.display = "block";
            finalScoreEl.innerText = score;
        }

        function resetGame() {
            bird.y = 300; bird.v = 0; pipes = []; score = 0; frame = 0;
            active = true; scoreEl.innerText = 0; msgEl.style.display = "none";
            loop();
        }

        window.addEventListener('keydown', (e) => { if(e.code === 'Space') bird.v = bird.jump; });
        canvas.addEventListener('touchstart', (e) => { e.preventDefault(); bird.v = bird.jump; });
        canvas.addEventListener('mousedown', () => { bird.v = bird.jump; });

        loop();
    </script>
</body>
</html>
