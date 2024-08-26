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
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: black;
            color: white;
        }
        .container {
            text-align: center;
            position: relative;
            padding: 20px;
            top: -10px;
        }
        h1 {
            color: yellow;
            font-family: 'Verdana', sans-serif;
            font-size: 48px;
            margin-bottom: 50px;
        }
        .graphics {
            position: absolute;
            top: 55%;
            transform: translateY(-50%);
            width: 340px;
        }
        .graphics.left {
            left: 150px; /* Bilder weiter zur Mitte verschieben */
        }
        .graphics.right {
            right: 150px; /* Bilder weiter zur Mitte verschieben */
        }
        .wheel-container {
            position: relative;
            width: 350px;
            height: 350px;
            margin: 0 auto;
        }
        .wheel {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background-color: #444444;
            border: 15px solid #ff6600; /* Kräftigerer Rand */
            position: relative;
            box-shadow: 0 0 20px rgba(255, 102, 0, 0.8);
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
            transform: translate(-50%, -50%) rotate(-36deg);
            transform-origin: 0% 0%;
            text-align: center;
            font-size: 18px;
            font-family: 'Verdana', sans-serif;
            font-weight: bold;
            color: white;
        }
        .segment:nth-child(1) {
            background-color: #ff6666;
            transform: rotate(0deg);
        }
        .segment:nth-child(2) {
            background-color: #ff3333;
            transform: rotate(72deg);
        }
        .segment:nth-child(3) {
            background-color: #66b3ff;
            transform: rotate(144deg);
        }
        .segment:nth-child(4) {
            background-color: #85e085;
            transform: rotate(216deg);
        }
        .segment:nth-child(5) {
            background-color: #ffcc66;
            transform: rotate(288deg);
        }
        .pointer {
            position: absolute;
            top: -40px;
            left: 50%;
            width: 0; 
            height: 0; 
            border-left: 20px solid transparent;
            border-right: 20px solid transparent;
            border-bottom: 40px solid #ff4d4d; /* Auffälliger, größerer Pfeil */
            transform: translateX(-50%) rotate(180deg);
            z-index: 20;
        }
        .timer {
            margin-top: 40px; /* Timer leicht nach unten verschieben */
            font-size: 24px;
            color: yellow;
            display: none;
        }
        button {
            padding: 15px 30px;
            font-size: 22px;
            background-color: #ff4d4d;
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            margin-top: 50px;
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.5);
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
    <div class="graphics left">
        <img src="2.jpg" alt="Linkes Bild" width="340">
    </div>
    <div class="container">
        <h1>Saller Enterprises Wheel of Drinks</h1>
        <div class="wheel-container">
            <div class="pointer"></div>
            <div class="wheel" id="wheel">
                <div class="segment" style="transform: rotate(0deg);">
                    <div class="segment-label">Asbach</div>
                </div>
                <div class="segment" style="transform: rotate(72deg);">
                    <div class="segment-label">Show Boobs!</div>
                </div>
                <div class="segment" style="transform: rotate(144deg);">
                    <div class="segment-label">Sekt</div>
                </div>
                <div class="segment" style="transform: rotate(216deg);">
                    <div class="segment-label">Bier</div>
                </div>
                <div class="segment" style="transform: rotate(288deg);">
                    <div class="segment-label">Milch</div>
                </div>
            </div>
        </div>
        <div class="timer" id="timer">9</div> <!-- Timeranzeige -->
        <button id="spinButton" onclick="spinWheel()">Spin the Wheel</button>
    </div>
    <div class="graphics right">
        <img src="2.jpg" alt="Rechtes Bild" width="340">
    </div>

    <script>
        const segments = ["Asbach", "Show Boobs!", "Sekt", "Bier", "Milch"];
        const timerElement = document.getElementById('timer');

        function spinWheel() {
            const wheel = document.getElementById('wheel');
            const spinButton = document.getElementById('spinButton');

            // Deaktiviert den Button, während das Rad sich dreht
            spinButton.disabled = true;

            // Zufällige Startposition für jede Drehung
            const startRotation = Math.floor(Math.random() * 360);
            wheel.style.transform = `rotate(${startRotation}deg)`;
            wheel.style.transition = 'none';

            setTimeout(() => {
                const initialRotation = -180 * 5; // Halbe Geschwindigkeit für die ersten 5 Sekunden
                const slowRotation = -360 * 2; // Verlangsamt bis 0,5 RPM über 5 Sekunden
                const randomStop = Math.floor(Math.random() * 800) + 200; // Zufälliger Stopp
                const totalRotation = startRotation + initialRotation + slowRotation;
                const duration = 5 + randomStop / 1000;

                // Setzt die Rotation und die Übergangsdauer
                wheel.style.transition = `transform ${duration}s ease-out`;
                wheel.style.transform = `rotate(${totalRotation}deg)`;

                // Sobald das Rad zum Stillstand kommt, wird der Timer gestartet
                setTimeout(() => {
                    let remainingTime = 9;
                    timerElement.style.display = 'block'; // Timer anzeigen

                    const countdownInterval = setInterval(() => {
                        remainingTime--;
                        if (remainingTime <= 0) {
                            clearInterval(countdownInterval);
                            timerElement.style.display = 'none'; // Blendet den Timer bei 0 aus
                        } else {
                            timerElement.innerText = remainingTime;
                        }
                    }, 1000);

                    // Button nach 9 Sekunden wieder aktivieren
                    setTimeout(() => {
                        spinButton.disabled = false;
                    }, 9000);
                }, duration * 1000);
            }, 50); // Kurze Verzögerung, um das Zurücksetzen der Transition zu ermöglichen
        }
    </script>
</body>
</html>
