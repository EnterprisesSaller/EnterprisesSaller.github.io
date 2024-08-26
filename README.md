<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Saller Enterprises Wheel of Drinks</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: black;
            color: white;
            overflow: hidden;
        }
        .container {
            text-align: center;
            position: relative;
            width: 80%;
            max-width: 800px;
            padding: 5vh 0;
        }
        .graphics {
            position: relative;
            top: -15vh;
            width: 55vw; /* Bildgröße um 10% vergrößert */
            max-width: 330px; /* Maximale Breite des Bildes */
            margin: 0 auto;
        }
        .wheel-container {
            position: relative;
            width: 50vw;
            height: 50vw;
            margin: 0 auto;
        }
        .wheel {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background-color: #444444;
            border: 2vw solid #ff6600;
            position: relative;
            box-shadow: 0 0 2vw rgba(255, 102, 0, 0.8);
        }
        .segment {
            position: absolute;
            width: 50%;
            height: 50%;
            top: 50%;
            left: 50%;
            transform-origin: 0% 0%;
            clip-path: polygon(0% 0%, 100% 0%, 50% 100%);
        }
        .segment-label {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            transform-origin: 0% 0%;
            text-align: center;
            font-size: 3.5vw;
            font-family: 'Verdana', sans-serif;
            font-weight: bold;
            color: white;
        }

        /* Segment Styles */
        .segment-small {
            clip-path: polygon(0% 0%, 100% 0%, 30% 100%);
        }

        .segment-large {
            clip-path: polygon(0% 0%, 100% 0%, 100% 100%);
        }

        .segment:nth-child(1) {
            background-color: #ff6666;
            transform: rotate(0deg);
        }
        .segment:nth-child(2) {
            background-color: #ff3333;
            transform: rotate(54deg);
        }
        .segment:nth-child(3) {
            background-color: #66b3ff;
            transform: rotate(108deg);
        }
        .segment:nth-child(4) {
            background-color: #85e085;
            transform: rotate(162deg);
        }
        .segment:nth-child(5) {
            background-color: #ffcc66;
            transform: rotate(216deg);
        }
        .segment:nth-child(6) {
            background-color: #ffa07a;
            transform: rotate(270deg);
        }
        .segment:nth-child(7) {
            background-color: #ff69b4;
            transform: rotate(324deg);
        }
        .pointer {
            position: absolute;
            top: -5vw;
            left: 50%;
            width: 0;
            height: 0;
            border-left: 3vw solid transparent;
            border-right: 3vw solid transparent;
            border-bottom: 6vw solid #ff4d4d;
            transform: translateX(-50%) rotate(180deg);
            z-index: 20;
        }
        .timer {
            margin-top: 5vh;
            font-size: 5vw;
            color: yellow;
            display: none;
        }
        button {
            padding: 2vw 4vw;
            font-size: 5vw;
            background-color: #ff4d4d;
            color: white;
            border: none;
            border-radius: 2vw;
            cursor: pointer;
            margin-top: 5vh;
            box-shadow: 0 0.5vw 1vw rgba(0, 0, 0, 0.5);
        }
        button:disabled {
            background-color: #ccc;
            cursor: not-allowed;
            box-shadow: none;
        }
        button:hover:enabled {
            background-color: #ff3333;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="graphics">
            <img src="2.jpg" alt="Bild" style="width: 100%;">
        </div>
        <div class="wheel-container">
            <div class="pointer"></div>
            <div class="wheel" id="wheel">
                <div class="segment segment-small" style="transform: rotate(0deg);">
                    <div class="segment-label">Show Boobs!</div>
                </div>
                <div class="segment segment-large" style="transform: rotate(36deg);">
                    <div class="segment-label">2 Schnaps</div>
                </div>
                <div class="segment segment-large" style="transform: rotate(108deg);">
                    <div class="segment-label">Schnaps</div>
                </div>
                <div class="segment segment-large" style="transform: rotate(180deg);">
                    <div class="segment-label">Milch 43</div>
                </div>
                <div class="segment segment-large" style="transform: rotate(252deg);">
                    <div class="segment-label">Hüttchen</div>
                </div>
                <div class="segment segment-small" style="transform: rotate(324deg);">
                    <div class="segment-label">Wasser</div>
                </div>
                <div class="segment segment-small" style="transform: rotate(360deg);">
                    <div class="segment-label">5€ fürs Team</div>
                </div>
            </div>
        </div>
        <div class="timer" id="timer">9</div>
        <button id="spinButton" onclick="spinWheel()">Spin the Wheel</button>
    </div>

    <script>
        const segments = ["Show Boobs!", "2 Schnaps", "Schnaps", "Milch 43", "Hüttchen", "Wasser", "5€ fürs Team"];
        const timerElement = document.getElementById('timer');

        function spinWheel() {
            const wheel = document.getElementById('wheel');
            const spinButton = document.getElementById('spinButton');

            spinButton.disabled = true;

            const startRotation = Math.floor(Math.random() * 360);
            wheel.style.transform = `rotate(${startRotation}deg)`;
            wheel.style.transition = 'none';

            setTimeout(() => {
                const initialRotation = -180 * 5;
                const slowRotation = -360 * 2;
                const randomStop = Math.floor(Math.random() * 800) + 200;
                const totalRotation = startRotation + initialRotation + slowRotation;
                const duration = 5 + randomStop / 1000;

                wheel.style.transition = `transform ${duration}s ease-out`;
                wheel.style.transform = `rotate(${totalRotation}deg)`;

                setTimeout(() => {
                    let remainingTime = 9;
                    timerElement.style.display = 'block';

                    const countdownInterval = setInterval(() => {
                        remainingTime--;
                        if (remainingTime <= 0) {
                            clearInterval(countdownInterval);
                            timerElement.style.display = 'none';
                        } else {
                            timerElement.innerText = remainingTime;
                        }
                    }, 1000);

                    setTimeout(() => {
                        spinButton.disabled = false;
                    }, 9000);
                }, duration * 1000);
            }, 50);
        }
    </script>
</body>
</html>
