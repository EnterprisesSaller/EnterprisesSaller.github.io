<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Partyspiel für 2 Spieler</title>
    <style>
        body {
            background-color: black;
            color: white;
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
        }

        img {
            width: 150%;
            max-width: 900px;
            height: auto;
            margin-top: 20px;
        }

        #game-container {
            margin-top: 20px;
            padding: 0 20px;
            position: relative;
        }

        #header-img {
            width: 80%;
            max-width: 300px;
            height: auto;
            margin-top: 20px;
        }

        #gluecksrad-bild {
            width: 80%;
            max-width: 300px;
            height: auto;
            border-radius: 50%;
            margin-top: 20px;
            position: relative;
            z-index: 1;
        }

        #pointer-container {
            position: absolute;
            top: 50%;
            left: 50%;
            width: 100%;
            height: 100%;
            transform: translate(-50%, -50%);
            z-index: 1000;
            pointer-events: none;
        }

        #pointer {
            width: 0;
            height: 0;
            border-left: 15px solid transparent;
            border-right: 15px solid transparent;
            border-bottom: 30px solid red;
            position: absolute;
            transform-origin: 150px 150px; /* Kreisen um die Mitte des Bildes */
        }

        .wuerfel-container {
            display: flex;
            justify-content: center;
            margin: 10px 0;
        }

        .wuerfel {
            width: 60px;
            height: 60px;
            margin: 10px;
            background-color: white;
            color: black;
            font-size: 24px;
            line-height: 60px;
            text-align: center;
            border-radius: 10px;
            border: 2px solid black;
        }

        .dots {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            grid-template-rows: repeat(3, 1fr);
            gap: 5px;
            width: 100%;
            height: 100%;
        }

        .dot {
            width: 10px;
            height: 10px;
            background-color: black;
            border-radius: 50%;
            place-self: center;
        }

        .btn {
            background-color: #555;
            color: white;
            padding: 10px 20px;
            border: none;
            cursor: pointer;
            font-size: 16px;
            margin-top: 20px;
            width: 100%;
            max-width: 200px;
            display: block;
            margin-left: auto;
            margin-right: auto;
        }

        .btn:disabled {
            background-color: #333;
            cursor: not-allowed;
        }

        .btn:hover:not(:disabled) {
            background-color: #777;
        }

        #result {
            margin: 20px auto;
            font-size: 16px;
        }

        #timer {
            font-size: 18px;
            margin-top: 20px;
        }
    </style>
</head>
<body>

    <img src="2.jpg" alt="Bildname 2" id="header-img">

    <div id="game-container">
        
        <div id="pointer-container">
            <div id="pointer"></div>
        </div>
        
        <img src="3.jpeg" alt="Glücksrad" id="gluecksrad-bild"> <!-- Glücksrad-Bild -->
        
        <button class="btn" id="gluecksradBtn" onclick="startGame()">Spin the Wheel</button>
        
        <div class="wuerfel-container">
            <div class="wuerfel" id="wuerfel1-1">
                <div class="dots" id="dots1-1"></div>
            </div>
            <div class="wuerfel" id="wuerfel1-2">
                <div class="dots" id="dots1-2"></div>
            </div>
        </div>
        <button class="btn" id="wuerfel1Btn" onclick="wuerfeln1()" disabled>Roll</button>
        
        <div class="wuerfel-container">
            <div class="wuerfel" id="wuerfel2-1">
                <div class="dots" id="dots2-1"></div>
            </div>
            <div class="wuerfel" id="wuerfel2-2">
                <div class="dots" id="dots2-2"></div>
            </div>
        </div>
        <button class="btn" id="wuerfel2Btn" onclick="wuerfeln2()" disabled>Roll</button>
        
        <div id="result">Wer gewinnt?</div>
        
        <div id="timer"></div>
    </div>

    <script>
        let player1Result = 0;
        let player2Result = 0;
        let timer = null;

        function startGame() {
            disableButtons();
            drehePointer();
        }

        function disableButtons() {
            document.getElementById('gluecksradBtn').disabled = true;
            document.getElementById('wuerfel1Btn').disabled = true;
            document.getElementById('wuerfel2Btn').disabled = true;
        }

        function enableButtons() {
            document.getElementById('gluecksradBtn').disabled = false;
            document.getElementById('wuerfel1Btn').disabled = false;
            document.getElementById('wuerfel2Btn').disabled = false;
        }

        function drehePointer() {
            const pointer = document.getElementById('pointer');
            const spinDuration = Math.random() * (6 - 5) + 5; // Zufällige Dauer zwischen 5 und 6 Sekunden
            const rotations = 30 * spinDuration; // 30 Umdrehungen pro Minute (RPM)

            pointer.style.transition = `transform ${spinDuration}s linear`;
            pointer.style.transform = `rotate(${rotations * 360}deg)`;

            setTimeout(() => {
                pointer.style.transition = 'transform 2s ease-out';
                const finalRotation = Math.floor(Math.random() * 360);
                pointer.style.transform = `rotate(${rotations * 360 + finalRotation}deg)`;

                setTimeout(() => {
                    document.getElementById('wuerfel1Btn').disabled = false;
                }, 2000);

            }, spinDuration * 1000);
        }

        function wuerfeln1() {
            disableButtons();
            animateDice('wuerfel1-1', 'dots1-1');
            animateDice('wuerfel1-2', 'dots1-2', function() {
                player1Result = showDiceResult('wuerfel1-1', 'dots1-1') + showDiceResult('wuerfel1-2', 'dots1-2');
                document.getElementById('wuerfel2Btn').disabled = false;
            });
        }

        function wuerfeln2() {
            disableButtons();
            animateDice('wuerfel2-1', 'dots2-1');
            animateDice('wuerfel2-2', 'dots2-2', function() {
                player2Result = showDiceResult('wuerfel2-1', 'dots2-1') + showDiceResult('wuerfel2-2', 'dots2-2');
                zeigeErgebnis();
            });
        }

        function animateDice(wuerfelId, dotsId, callback) {
            let iterations = 30;
            let interval = setInterval(function() {
                showRandomDots(dotsId);
                iterations--;
                if (iterations <= 0) {
                    clearInterval(interval);
                    if (callback) callback();
                }
            }, 100);
        }

        function showRandomDots(dotsId) {
            const patterns = [
                [4], // 1
                [0, 8], // 2
                [0, 4, 8], // 3
                [0, 2, 6, 8], // 4
                [0, 2, 4, 6, 8], // 5
                [0, 1, 2, 6, 7, 8] // 6
            ];
            const randomPattern = patterns[Math.floor(Math.random() * 6)];
            const dots = document.getElementById(dotsId);
            dots.innerHTML = '';
            for (let i = 0; i < 9; i++) {
                const dot = document.createElement('div');
                if (randomPattern.includes(i)) dot.className = 'dot';
                dots.appendChild(dot);
            }
        }

        function showDiceResult(wuerfelId, dotsId) {
            const dots = document.getElementById(dotsId);
            const result = Math.floor(Math.random() * 6) + 1;
            dots.innerHTML = '';
            const patterns = [
                [4], // 1
                [0, 8], // 2
                [0, 4, 8], // 3
                [0, 2, 6, 8], // 4
                [0, 2, 4, 6, 8], // 5
                [0, 1, 2, 6, 7, 8] // 6
            ];
            const finalPattern = patterns[result - 1];
            for (let i = 0; i < 9; i++) {
                const dot = document.createElement('div');
                if (finalPattern.includes(i)) dot.className = 'dot';
                dots.appendChild(dot);
            }
            return result;
        }

        function zeigeErgebnis() {
            let resultText = '';
            if (player1Result > player2Result) {
                resultText = 'Spieler 1 gewinnt!';
                startTimer();
            } else if (player1Result < player2Result) {
                resultText = 'Spieler 2 gewinnt!';
                startTimer();
            } else {
                resultText = 'Unentschieden! Nochmal würfeln!';
                document.getElementById('wuerfel1Btn').disabled = false;
                document.getElementById('wuerfel2Btn').disabled = true;
            }
            document.getElementById('result').textContent = resultText;
            if (player1Result !== player2Result) {
                enableButtons(); // Nur bei einem eindeutigen Ergebnis die Buttons wieder aktivieren
            }
        }

        function startTimer() {
            let countdown = 19;  // Timer auf 19 Sekunden eingestellt
            document.getElementById('timer').textContent = 'Nächstes Spiel in: ' + countdown + ' Sekunden';
            
            timer = setInterval(function() {
                countdown--;
                if (countdown > 0) {
                    document.getElementById('timer').textContent = 'Nächstes Spiel in: ' + countdown + ' Sekunden';
                } else {
                    clearInterval(timer);
                    document.getElementById('timer').textContent = '';
                    resetGame();
                }
            }, 1000);
        }

        function resetGame() {
            document.getElementById('pointer').style.transform = 'rotate(0deg)';
            document.getElementById('dots1-1').innerHTML = '';
            document.getElementById('dots1-2').innerHTML = '';
            document.getElementById('dots2-1').innerHTML = '';
            document.getElementById('dots2-2').innerHTML = '';
            document.getElementById('result').textContent = 'Wer gewinnt?';
            enableButtons(); // Buttons nach dem Reset wieder aktivieren
        }
    </script>
</body>
</html>
