
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>七彩灯</title>
    <style>
        :root {
            --lamp-size: 70px;
            --pulse-duration: 1.4s;
        }

        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            background: radial-gradient(circle at center, #1a1a1a, #000);
            font-family: system-ui, sans-serif;
        }

        .galaxy {
            display: flex;
            gap: 2rem;
            padding: 3rem;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 2rem;
            backdrop-filter: blur(12px);
            box-shadow: 0 0 40px rgba(100, 200, 255, 0.1);
        }

        .lamp {
            width: var(--lamp-size);
            height: var(--lamp-size);
            border-radius: 50%;
            animation: pulse var(--pulse-duration) ease-in-out infinite;
            animation-play-state: paused;
            box-shadow: 0 0 20px currentColor;
            transition: background-color 0.3s;
        }

        @keyframes pulse {
            0%, 100% {
                transform: scale(1);
                opacity: 1;
            }
            50% {
                transform: scale(1.25);
                opacity: 0.8;
            }
        }

        .controls {
            margin-top: 3rem;
            display: flex;
            gap: 1.5rem;
        }

        .btn {
            padding: 1rem 2.5rem;
            font-size: 1.1rem;
            border: none;
            border-radius: 2rem;
            cursor: pointer;
            transition: all 0.3s ease;
            background: linear-gradient(145deg, #00c6ff, #0072ff);
            color: white;
            text-shadow: 0 1px 2px rgba(0,0,0,0.2);
            box-shadow: 0 4px 20px rgba(0, 114, 255, 0.3);
        }

        .btn--stop {
            background: linear-gradient(145deg, #ff4b4b, #ff0000);
            box-shadow: 0 4px 20px rgba(255, 0, 0, 0.3);
        }

        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 25px rgba(0, 114, 255, 0.5);
        }

        .btn--stop:hover {
            box-shadow: 0 6px 25px rgba(255, 0, 0, 0.5);
        }
    </style>
</head>
<body>
    <div class="galaxy">
        <div class="lamp"></div>
        <div class="lamp"></div>
        <div class="lamp"></div>
        <div class="lamp"></div>
        <div class="lamp"></div>
        <div class="lamp"></div>
        <div class="lamp"></div>
    </div>

    <div class="controls">
        <button class="btn" id="start">启动</button>
        <button class="btn btn--stop" id="stop">停止</button>
    </div>

    <script>
        const colors = [
            '#FF6B6B', '#FFD93D', '#6CBF6D',
            '#4ECDC4', '#45B7D1', '#9B59B6',
            '#FF7F50'
        ];
        
        const lamps = document.querySelectorAll('.lamp');
        let isRunning = false;
        let animationFrame = null;
        let startTime = null;

        function updateColors(timestamp) {
            if (!startTime) startTime = timestamp;
            const elapsed = timestamp - startTime;
            const timeIndex = Math.floor(elapsed / 1000);

            lamps.forEach((lamp, index) => {
                const colorIndex = (timeIndex + index) % 7;
                lamp.style.backgroundColor = colors[colorIndex];
                lamp.style.boxShadow = `0 0 30px ${colors[colorIndex]}`;
            });

            animationFrame = requestAnimationFrame(updateColors);
        }

        document.getElementById('start').addEventListener('click', () => {
            if (!isRunning) {
                isRunning = true;
                startTime = null;
                lamps.forEach(lamp => {
                    lamp.style.animationPlayState = 'running';
                });
                animationFrame = requestAnimationFrame(updateColors);
            }
        });

        document.getElementById('stop').addEventListener('click', () => {
            if (isRunning) {
                isRunning = false;
                cancelAnimationFrame(animationFrame);
                lamps.forEach(lamp => {
                    lamp.style.animationPlayState = 'paused';
                });
            }
        });
    </script>
</body>
</html>
