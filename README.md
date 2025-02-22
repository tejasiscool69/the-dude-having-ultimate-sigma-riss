# the-dude-having-ultimate-sigma-riss

https://github.com/tejasiscool69/the-dude-having-ultimate-sigma-riss.git
this is my game please download it and play in html format in notepad and save as .html in desktop - tejas




<!DOCTYPE html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Click the Target Game - Multi-Level</title>
    <style>
        body {
            text-align: center;
            background: linear-gradient(to right, #ff7e5f, #feb47b);
            font-family: Arial, sans-serif;<html lang="en">
        }
        h1 {
            color: white;
        }
        .game-container {
            position: relative;
            width: 80vw;
            height: 60vh;
            margin: auto;
            background: white;
            border: 5px solid black;
            overflow: hidden;
            position: relative;
        }
        .target {
            width: 50px;
            height: 50px;
            background: red;
            border-radius: 50%;
            position: absolute;
            cursor: pointer;
            box-shadow: 0 0 10px black;
        }
        .scoreboard {
            font-size: 24px;
            color: white;
            margin: 20px;
        }
        .play-again {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 18px;
            cursor: pointer;
            background-color: #28a745;
            color: white;
            border: none;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <h1>Click the Moving Target!</h1>
    <input type="text" id="username" placeholder="Enter your name">
    <button onclick="startGame()">Start Game</button>
    <div class="scoreboard">Level: <span id="level">1</span> | Score: <span id="score">0</span> | Time: <span id="timer">10</span>s</div>
    <div class="game-container" id="gameContainer"></div>
    <div id="leaderboard"></div>
    <audio id="clickSound" src="https://www.fesliyanstudios.com/play-mp3/4383"></audio>
    <audio id="levelUpSound" src="https://www.fesliyanstudios.com/play-mp3/4392"></audio>
    
    <script>
        let score = 0;
        let level = 1;
        let timeLeft = 10;
        const maxLevels = 5;
        let username = "";
        const gameContainer = document.getElementById("gameContainer");
        const scoreDisplay = document.getElementById("score");
        const timerDisplay = document.getElementById("timer");
        const levelDisplay = document.getElementById("level");
        const leaderboard = document.getElementById("leaderboard");
        const clickSound = document.getElementById("clickSound");
        const levelUpSound = document.getElementById("levelUpSound");
        let scores = [];
        
        function randomPosition() {
            let x = Math.floor(Math.random() * (gameContainer.clientWidth - 50));
            let y = Math.floor(Math.random() * (gameContainer.clientHeight - 50));
            return { x, y };
        }
        
        function createTarget() {
            const target = document.createElement("div");
            target.classList.add("target");
            let pos = randomPosition();
            target.style.left = pos.x + "px";
            target.style.top = pos.y + "px";
            
            target.addEventListener("click", function() {
                score++;
                scoreDisplay.textContent = score;
                clickSound.play();
                target.remove();
                createTarget();
            });
            
            gameContainer.appendChild(target);
        }
        
        function startGame() {
            username = document.getElementById("username").value || "Player";
            document.getElementById("username").style.display = "none";
            document.querySelector("button").style.display = "none";
            startLevel();
        }
        
        function startLevel() {
            gameContainer.innerHTML = "";
            createTarget();
            timeLeft = 10 - (level - 1) * 2;
            timerDisplay.textContent = timeLeft;
            
            const timer = setInterval(() => {
                timeLeft--;
                timerDisplay.textContent = timeLeft;
                
                if (timeLeft <= 0) {
                    clearInterval(timer);
                    if (level < maxLevels) {
                        levelUp();
                    } else {
                        endGame();
                    }
                }
            }, 1000);
        }
        
        function levelUp() {
            levelUpSound.play();
            level++;
            levelDisplay.textContent = level;
            gameContainer.innerHTML = `<h2>Congratulations! You completed Level ${level - 1}. Your score: ${score}</h2>`;
            setTimeout(() => {
                gameContainer.innerHTML = "";
                startLevel();
            }, 2000);
        }
        
        function endGame() {
            scores.push({ name: username, score: score });
            scores.sort((a, b) => b.score - a.score);
            gameContainer.innerHTML = `<h2>Game Over! Your Total Score: ${score}</h2>`;
            updateLeaderboard();
            
            const playAgainBtn = document.createElement("button");
            playAgainBtn.textContent = "Play Again";
            playAgainBtn.classList.add("play-again");
            playAgainBtn.onclick = restartGame;
            gameContainer.appendChild(playAgainBtn);
        }
        
        function restartGame() {
            score = 0;
            level = 1;
            scoreDisplay.textContent = score;
            levelDisplay.textContent = level;
            startGame();
        }
        
        function updateLeaderboard() {
            leaderboard.innerHTML = "<h2>Leaderboard</h2>" + scores.map(s => `<p>${s.name}: ${s.score}</p>`).join("");
        }
    </script>
</body>
</html>
