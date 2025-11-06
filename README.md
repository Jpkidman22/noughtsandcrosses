<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Noughts and Crosses</title>
<style>
  body { font-family: sans-serif; text-align: center; background: #fafafa; }
  h1 { margin-top: 20px; }
  #board { display: grid; grid-template-columns: repeat(3, 100px); 
           grid-gap: 5px; justify-content: center; margin: 30px auto; }
  .cell { width: 100px; height: 100px; font-size: 2.5em; 
          display: flex; align-items: center; justify-content: center;
          background: #fff; border: 2px solid #333; cursor: pointer; }
  #status { font-size: 1.2em; margin-top: 20px; }
  button { margin-top: 10px; padding: 8px 16px; font-size: 1em; }
</style>
</head>
<body>
<h1>Noughts and Crosses</h1>
<div id="board"></div>
<div id="status"></div>
<button onclick="resetGame()">Restart</button>

<script>
const board = document.getElementById('board');
const statusText = document.getElementById('status');
let cells = Array(9).fill('');
let gameOver = false;

function drawBoard() {
  board.innerHTML = '';
  cells.forEach((val, i) => {
    const cell = document.createElement('div');
    cell.className = 'cell';
    cell.textContent = val;
    cell.onclick = () => playerMove(i);
    board.appendChild(cell);
  });
}

function playerMove(i) {
  if (gameOver || cells[i]) return;
  cells[i] = 'X';
  drawBoard();
  if (checkWinner('X')) return endGame('You win!');
  if (cells.every(c => c)) return endGame("It's a draw!");
  setTimeout(computerMove, 400);
}

function computerMove() {
  let empty = cells.map((v, i) => v ? null : i).filter(v => v !== null);
  if (!empty.length) return;
  let choice = empty[Math.floor(Math.random() * empty.length)];
  cells[choice] = 'O';
  drawBoard();
  if (checkWinner('O')) return endGame('Computer wins!');
  if (cells.every(c => c)) return endGame("It's a draw!");
}

function checkWinner(p) {
  const wins = [
    [0,1,2],[3,4,5],[6,7,8],
    [0,3,6],[1,4,7],[2,5,8],
    [0,4,8],[2,4,6]
  ];
  return wins.some(line => line.every(i => cells[i] === p));
}

function endGame(msg) {
  gameOver = true;
  statusText.textContent = msg;
}

function resetGame() {
  cells = Array(9).fill('');
  gameOver = false;
  statusText.textContent = '';
  drawBoard();
}

drawBoard();
</script>
</body>
</html>

