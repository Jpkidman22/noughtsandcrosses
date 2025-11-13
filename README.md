<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Noughts and Crosses</title>
<style>
  body { font-family: sans-serif; text-align: center; background: #fafafa; }
  h1 { margin-top: 20px; }
  #board {
    display: grid; grid-template-columns: repeat(3, 100px);
    gap: 5px; justify-content: center; margin: 30px auto;
  }
  .cell {
    width: 100px; height: 100px; background: #fff;
    border: 2px solid #000; font-size: 2.5em;
    display: flex; justify-content: center; align-items: center;
    cursor: pointer;
  }
  #message { font-size: 1.2em; margin-top: 15px; }
  button { margin-top: 10px; padding: 5px 15px; font-size: 1em; }
</style>
</head>
<body>
<h1>Noughts and Crosses</h1>
<div id="board"></div>
<div id="message"></div>
<button onclick="reset()">Restart</button>

<script>
const board = document.getElementById('board');
const message = document.getElementById('message');
let cells = Array(9).fill('');
let gameOver = false;

function render() {
  board.innerHTML = '';
  cells.forEach((val, i) => {
    const div = document.createElement('div');
    div.className = 'cell';
    div.textContent = val;
    div.onclick = () => playerMove(i);
    board.appendChild(div);
  });
}

function playerMove(i) {
  if (cells[i] || gameOver) return;
  cells[i] = 'X';
  if (checkWin('X')) return end('You win!');
  if (cells.every(c => c)) return end('Draw!');
  computerMove();
}

function computerMove() {
  let empty = cells.map((v,i)=>v?null:i).filter(v=>v!==null);
  let move = empty[Math.floor(Math.random()*empty.*]()
