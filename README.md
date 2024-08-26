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
            width: 150%; /* 50% größer */
            max-width: 900px;
            height: auto;
            margin-top: -20px; /* Weiter nach oben verschoben */
        }

        #game-container {
            margin-top: 10px;
            padding: 0 20px;
        }

        #gluecksrad-container {
            position: relative;
            width: 100%;
            max-width: 300px;
            height: 300px;
            margin: 0 auto;
            margin-top: -40px; /* Weiter nach oben verschoben */
        }

        #gluecksrad {
            width: 100%;
            height: 100%;
            position: relative;
            border-radius: 50%;
            border: 5px solid white;
            transform: rotate(0deg);
            overflow: hidden;
        }

        .segment {
            position: absolute;
            width: 50%;
            height: 50%;
            background-color: white;
            clip-path: polygon(50% 50%, 100% 0%, 100% 100%);
            transform-origin: 50% 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            text-align: center;
            color: white;
            font-size: 12px;
            font-weight: bold;
        }

        .segment:nth-child(1) {
            background-color: red;
            transform: rotate(0deg);
        }

        .segment:nth-child(2) {
            background-color: blue;
            transform: rotate(72deg);
        }

        .segment:nth-child(3) {
            background-color: green;
            transform: rotate(144deg);
        }

        .segment:nth-child(4) {
            background-color: yellow;
            color: black;
            transform: rotate(216deg);
        }

        .segment:nth-child(5) {
            background-color: purple;
            transform: rotate(288deg);
        }

        .segment span {
            transform: rotate(-36deg);
            display: block;
        }

        #pointer {
            width: 0;
            height: 0;
            border-left: 15px solid transparent;
            border-right: 15px solid transparent;
            border-top: 30px solid red;
            position: absolute;
            top: -20px;
            left: calc(50% - 15px);
            z-index: 1000;
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

        #result {
            margin: 20px auto;
            font-size: 16px;
        }

        .btn {
            background-color: #555;
            color: white;
            padding: 10px 20px;
            border: none;
            cursor: pointer;
            font-size: 16px;
            margin-top: 10px;
            width: 100%;
            max-width: 200px;
        }

        .btn:disabled {
            background-color: #333;
            cursor: not-allowed;
        }

        .btn:hover:not(:disabled) {
            background-color: #777;
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
            <div id="pointer"></div>
            <div id="gluecksrad">
                <div class="segment"><span>ein Schnaps</span></div>
                <div class="segment"><span>zwei Schnaps</span></div>
                <div class="segment"><span>Milch 43</span></div>
                <div class="segment"><span>Boobs out!</span></div>
                <div class="segment"><span>Hüttchen</span></div>
            </div>
        </div>
        <button class="btn" id="gluecksradBtn" onclick="dreheGluecksrad()">Spin the Wheel</button>
        
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
        let segment = 0;
        let player1Result = 0;
        let player2Result = 0;
        let timer = null;

        function dreheGluecksrad() {
            let rotation = Math.floor(Math.random() * 3600) + 360; // Mehrfache Umdrehungen für den Effekt
            document.getElementById('gluecksrad').style.transform = 'rotate(' + rotation + 'deg)';

            setTimeout(function() {
                segment = Math.floor(Math.random() * 5) + 1;
                document.getElementById('gluecksradBtn').disabled = true;
                document.getElementById('wuerfel1Btn').disabled = false;
            }, 4000); // Zeit, bis das Glücksrad zum Stillstand kommt
        }

        function wuerfeln1() {
            animateDice('wuerfel1-1', 'dots1-1');
            animateDice('wuerfel1-2', 'dots1-2', function() {
                player1Result = showDiceResult('wuerfel1-1', 'dots1-1') + showDiceResult('wuerfel1-2', 'dots1-2');
                document.getElementById('wuerfel1Btn').disabled = true;
                document.getElementById('wuerfel2Btn').disabled = false;
            });
        }

        function wuerfeln2() {
            animateDice('wuerfel2-1', 'dots2-1');
            animateDice('wuerfel2-2', 'dots2-2', function() {
                player2Result = showDiceResult('wuerfel2-1', 'dots2-1') + showDiceResult('wuerfel2-2', 'dots2-2');
                document.getElementById('wuerfel2Btn').disabled = true;
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
            }, 100); // Schnelle Änderung für 3 Sekunden (30 Iterationen à 100ms)
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
        }

        function startTimer() {
            let countdown = 9;
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
            document.getElementById('gluecksrad').style.transform = 'rotate(0deg)';
            document.getElementById('dots1-1').innerHTML = '';
            document.getElementById('dots1-2').innerHTML = '';
            document.getElementById('dots2-1').innerHTML = '';
            document.getElementById('dots2-2').innerHTML = '';
            document.getElementById('result').textContent = 'Wer gewinnt?';
            document.getElementById('gluecksradBtn').disabled = false;
            document.getElementById('wuerfel1Btn').disabled = true;
            document.getElementById('wuerfel2Btn').disabled = true;
        }
    </script>
</body>
</html>
