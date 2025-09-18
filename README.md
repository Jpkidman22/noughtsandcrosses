<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Noughts and Crosses</title>
  <style>
    body {
      font-family: sans-serif;
      text-align: center;
      margin-top: 50px;
    }
    h1 { margin-bottom: 10px; }
    #board {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      grid-gap: 5px;
      justify-content: center;
      margin: 20px auto;
    }
    .cell {
      width: 100px;
      height: 100px;
      font-size: 48px;
      display: flex;
      justify-content: center;
      align-items: center;
      background: #eee;
      cursor: pointer;
    }
    .cell:hover {
      background: #ddd;
    }
    #status {
      margin-top: 20px;
      font-size: 20px;
    }
    button {
      margin-top: 10px;
      padding: 8px 16px;
      font-size: 16px;
    }
  </style>
</head>
<body>
  <h1>Noughts and Crosses</h1>
  <div id="board"></div>
  <div id="status"></div>
  <button onclick="resetGame()">Restart</button>

  <script>
    const boardEl = document.getElementById('board');
    const statusEl = document.getElementById('status');
    let board = ['', '', '', '', '', '', '', '', ''];
    let gameOver = false;

    function renderBoard() {
      boardEl.innerHTML = '';
      board.forEach((cell, i) => {
        const div = document.createElement('div');
        div.className = 'cell';
        div.textContent = cell;
        div.onclick = () => playerMove(i);
        boardEl.appendChild(div);
      });
    }

    function playerMove(i) {
      if (board[i] || gameOver) return;
      board[i] = 'X';
      renderBoard();
      if (checkWin('X')) return endGame('You win!');
      if (board.every(c => c)) return endGame("It's a draw!");
      setTimeout(computerMove, 300);
    }

    function computerMove() {
      if (gameOver) return;
      const empty = board.map((c, i) => c === '' ? i : null).filter(i => i !== null);
      const move = empty[Math.floor(Math.random() * empty.length)];
      board[move] = 'O';
      renderBoard();
      if (checkWin('O')) return endGame('Computer wins!');
      if (board.every(c => c)) return endGame("It's a draw!");
    }

    function checkWin(player) {
      const winPatterns = [
        [0,1,2], [3,4,5], [6,7,8], // rows
        [0,3,6], [1,4,7], [2,5,8], // cols
        [0,4,8], [2,4,6]           // diags
      ];
      return winPatterns.some(p => 
        p.every(i => board[i] === player)
      );
    }

    function endGame(msg) {
      gameOver = true;
      statusEl.textContent = msg;
    }

    function resetGame() {
      board = ['', '', '', '', '', '', '', '', ''];
      gameOver = false;
      statusEl.textContent = '';
      renderBoard();
    }

    resetGame();
  </script>
</body>
</html>
