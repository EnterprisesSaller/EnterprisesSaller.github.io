
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

        #gluecksrad-container {
            position: relative;
            margin-top: 20px;
            width: 80%;
            max-width: 300px;
            height: auto;
            margin: 0 auto;
        }

        #gluecksrad-bild {
            width: 100%;
            border-radius: 50%;
        }

        #fixed-pointer {
            width: 0;
            height: 0;
            border-left: 15px solid transparent;
            border-right: 15px solid transparent;
            border-bottom: 30px solid red;
            position: absolute;
            top: -45px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 10;
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
        <div id="gluecksrad-container">
            <div id="fixed-pointer"></div> <!-- Fester Pfeil -->
            <img src="3.jpeg" alt="Glücksrad" id="gluecksrad-bild"> <!-- Glücksrad-Bild -->
        </div>
        
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
            dreheGluecksrad();
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

        function dreheGluecksrad() {
            const gluecksrad = document.getElementById('gluecksrad-bild');
            const spinDuration = 5; // 5 Sekunden schnelle Drehung
            const finalDuration = 2; // Verlangsamung für 2 Sekunden
            const rotations = 30 * spinDuration; // Schnelle Rotation

            gluecksrad.style.transition = `transform ${spinDuration}s linear`;
            gluecksrad.style.transform = `rotate(${rotations * 360}deg)`; // Schnelle Drehung

            setTimeout(() => {
                gluecksrad.style.transition = `transform ${finalDuration}s ease-out`;
                const finalRotation = Math.floor(Math.random() * 360);
                gluecksrad.style.transform = `rotate(${rotations * 360 + finalRotation}deg)`; // Verlangsamung zum Stillstand

                setTimeout(() => {
                    document.getElementById('wuerfel1Btn').disabled = false;
                }, finalDuration * 1000);

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
            document.getElementById('gluecksrad-bild').style.transform = 'rotate(0deg)';
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
