<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body {
            font-family: 'Arial', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #3498db;
        }

        #game-container {
            background-color: #ecf0f1;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
            overflow: hidden;
        }

        #board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-gap: 5px;
            justify-content: center;
            align-items: center;
            background-color: #fff;
            border: 2px solid #333;
            border-radius: 10px;
            padding: 10px;
        }

        .cell {
            width: 100px;
            height: 100px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            font-weight: bold;
            border: 2px solid #333;
            cursor: pointer;
            background-color: #eee;
            transition: background-color 0.3s ease;
        }

        .cell:hover {
            background-color: #ddd;
        }

        #resultPopup {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
            z-index: 999;
        }

        #restartButton {
            background-color: #4caf50;
            color: #fff;
            padding: 10px 20px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        #playerNames {
            text-align: center;
            margin: 20px 0;
        }

        input {
            padding: 10px;
            font-size: 16px;
            margin: 5px;
        }
    </style>
    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const board = document.getElementById('board');
            const cells = document.querySelectorAll('.cell');
            const resultPopup = document.getElementById('resultPopup');
            const restartButton = document.getElementById('restartButton');
            const resultText = document.getElementById('resultText');
            const playerNamesForm = document.getElementById('playerNamesForm');
            let currentPlayer = 'X';
            let gameBoard = ['', '', '', '', '', '', '', '', ''];
            let gameActive = true;
            let playerNames = ['', ''];

            cells.forEach(cell => {
                cell.addEventListener('click', () => cellClick(cell));
            });

            restartButton.addEventListener('click', restartGame);

            playerNamesForm.addEventListener('submit', function (event) {
                event.preventDefault();
                playerNames = [
                    document.getElementById('player1Name').value || 'Player 1',
                    document.getElementById('player2Name').value || 'Player 2'
                ];
                restartGame();
            });

            function cellClick(cell) {
                const index = Array.from(cells).indexOf(cell);
                if (gameBoard[index] === '' && gameActive) {
                    gameBoard[index] = currentPlayer;
                    cell.textContent = currentPlayer;
                    checkWinner();
                    currentPlayer = (currentPlayer === 'X') ? 'O' : 'X';
                }
            }

            function checkWinner() {
                const winningCombinations = [
                    [0, 1, 2], [3, 4, 5], [6, 7, 8],
                    [0, 3, 6], [1, 4, 7], [2, 5, 8],
                    [0, 4, 8], [2, 4, 6]
                ];

                for (const combo of winningCombinations) {
                    const [a, b, c] = combo;
                    if (gameBoard[a] !== '' && gameBoard[a] === gameBoard[b] && gameBoard[b] === gameBoard[c]) {
                        displayResult(`${playerNames[currentPlayer === 'X' ? 0 : 1]} wins!`);
                        gameActive = false;
                        return;
                    }
                }

                if (!gameBoard.includes('') && gameActive) {
                    displayResult('It\'s a draw!');
                    gameActive = false;
                }
            }

            function displayResult(message) {
                resultText.textContent = message;
                resultPopup.style.display = 'block';
            }

            function restartGame() {
                resultPopup.style.display = 'none';
                gameBoard = ['', '', '', '', '', '', '', '', ''];
                cells.forEach(cell => {
                    cell.textContent = '';
                });
                currentPlayer = 'X';
                gameActive = true;
            }
        });
    </script>
</head>
<body>
    <div id="game-container">
        <div id="board">
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
        </div>

        <div id="resultPopup">
            <p id="resultText"></p>
            <button id="restartButton">Restart Game</button>
        </div>

        <div id="playerNames">
            <form id="playerNamesForm">
                <label for="player1Name">Player 1 Name:</label>
                <input type="text" id="player1Name" placeholder="Enter name">

                <label for="player2Name">Player 2 Name:</label>
                <input type="text" id="player2Name" placeholder="Enter name">

                <button type="submit">Start Game</button>
            </form>
        </div>
    </div>
</body>
</html>
