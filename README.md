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
            margin-top: 20px; /* Bild leicht nach unten verschoben */
        }

        #game-container {
            margin-top: 20px;
            padding: 0 20px;
            position: relative;
        }

        #gluecksrad-container {
            position: relative;
            margin: 0 auto;
            width: 80%;
            max-width: 300px;
            height: 300px;
            background-image: url('3.jpeg');
            background-size: cover;
            background-position: center;
            border-radius: 50%;
            border: 5px solid white;
            overflow: hidden;
            transition: transform 6s ease-out;
            margin-top: -30px; /* Rad nach oben verschoben */
        }

        #pointer {
            width: 0;
            height: 0;
            border-left: 15px solid transparent;
            border-right: 15px solid transparent;
            border-bottom: 30px solid red; /* Pfeil zeigt nach unten */
            position: absolute;
            top: -40px; /* Pfeil über dem Rad */
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
            let rotation = Math.floor(Math.random() * 3600) + 720; // Mehrfache Umdrehungen für den Effekt
            document.getElementById('gluecksrad-container').style.transform = 'rotate(' + rotation + 'deg)';

            setTimeout(function() {
                document.getElementById('gluecksradBtn').disabled = true;
                document.getElementById('wuerfel1Btn').disabled = false;
            }, 6000); // Zeit, bis das Glücksrad zum Stillstand kommt
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
            document.getElementById('gluecksrad-container').style.transform = 'rotate(0deg)';
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
