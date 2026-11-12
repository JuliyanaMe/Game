<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Dragon Loves Chocolate</title>
  <style>
    :root {
      --sky: #dff7ff;
      --grass: #c8f5c3;
      --wall: #8b7d6b;
      --pink: #ffbfd8;
      --purple: #b58cff;
      --gold: #ffd166;
      --dragon: #7be0a8;
      --choco: #7b4b2a;
      --text: #3a2d2d;
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      min-height: 100vh;
      display: grid;
      place-items: center;
      background: linear-gradient(#dff7ff, #ffe6f2);
      font-family: Arial, sans-serif;
      color: var(--text);
    }

    .game-box {
      background: rgba(255,255,255,0.5);
      border: 4px solid #fff;
      border-radius: 22px;
      box-shadow: 0 18px 40px rgba(99, 63, 92, 0.18);
      padding: 20px 18px 14px;
      text-align: center;
    }

    h1 {
      margin: 0 0 8px;
      font-size: 2rem;
      letter-spacing: 1px;
    }

    .status {
      margin-bottom: 10px;
      font-size: 1rem;
      font-weight: bold;
      color: #6a4366;
    }

    .board {
      display: grid;
      grid-template-columns: repeat(8, 52px);
      grid-template-rows: repeat(8, 52px);
      gap: 6px;
      background: #f9f9f9;
      padding: 10px;
      border-radius: 18px;
      border: 3px solid #e7d9ff;
    }

    .cell {
      width: 52px;
      height: 52px;
      display: grid;
      place-items: center;
      border-radius: 12px;
      background: linear-gradient(#f2fff4, #dff7e9);
      font-size: 2rem;
      user-select: none;
    }

    .wall {
      background: linear-gradient(#b7a084, #8b7d6b);
    }

    .goal {
      background: linear-gradient(#ffebad, #ffd166);
    }

    .message {
      margin-top: 12px;
      min-height: 26px;
      font-weight: bold;
      color: #7a3b2e;
    }

    .controls {
      margin-top: 12px;
      font-size: 0.95rem;
      color: #5d4d69;
    }
  </style>
</head>
<body>
  <div class="game-box">
    <h1>🐉 Dragon Loves Chocolate 🍫</h1>
    <div class="status" id="status">Find the chocolate!</div>
    <div class="board" id="board"></div>
    <div class="message" id="message">Use arrow keys or WASD to move.</div>
    <div class="controls">Goal: reach the chocolate without hitting the rocks.</div>
  </div>

  <script>
    const boardSize = 8;
    const boardEl = document.getElementById("board");
    const statusEl = document.getElementById("status");
    const messageEl = document.getElementById("message");

    let dragon = { x: 0, y: 0 };
    let chocolate = { x: 6, y: 6 };
    let score = 0;

    const walls = [
      { x: 2, y: 1 }, { x: 3, y: 1 }, { x: 5, y: 1 },
      { x: 2, y: 3 }, { x: 5, y: 3 }, { x: 6, y: 3 },
      { x: 1, y: 5 }, { x: 3, y: 5 }, { x: 5, y: 5 },
      { x: 6, y: 5 }
    ];

    function isWall(x, y) {
      return walls.some(wall => wall.x === x && wall.y === y);
    }

    function renderBoard() {
      boardEl.innerHTML = "";
      for (let y = 0; y < boardSize; y++) {
        for (let x = 0; x < boardSize; x++) {
          const cell = document.createElement("div");
          cell.classList.add("cell");

          if (isWall(x, y)) {
            cell.classList.add("wall");
            cell.textContent = "🪨";
          } else if (dragon.x === x && dragon.y === y) {
            cell.textContent = "🐉";
          } else if (chocolate.x === x && chocolate.y === y) {
            cell.classList.add("goal");
            cell.textContent = "🍫";
          } else {
            cell.textContent = "";
          }

          boardEl.appendChild(cell);
        }
      }
    }

    function updateStatus() {
      statusEl.textContent = `Chocolate found: ${score}`;
    }

    function moveDragon(dx, dy) {
      const nextX = dragon.x + dx;
      const nextY = dragon.y + dy;

      if (
        nextX < 0 || nextX >= boardSize ||
        nextY < 0 || nextY >= boardSize ||
        isWall(nextX, nextY)
      ) {
        messageEl.textContent = "Oops! The dragon bumped into a rock. Try again!";
        return;
      }

      dragon.x = nextX;
      dragon.y = nextY;

      if (dragon.x === chocolate.x && dragon.y === chocolate.y) {
        score += 1;
        messageEl.textContent = "Yay! The dragon reached the chocolate! 🍫✨";
        updateStatus();
        setTimeout(() => {
          chocolate.x = Math.floor(Math.random() * boardSize);
          chocolate.y = Math.floor(Math.random() * boardSize);

          if (chocolate.x === dragon.x && chocolate.y === dragon.y) {
            chocolate.x = (chocolate.x + 2) % boardSize;
            chocolate.y = (chocolate.y + 1) % boardSize;
          }

          if (isWall(chocolate.x, chocolate.y)) {
            chocolate.x = 7;
            chocolate.y = 7;
          }

          messageEl.textContent = "New chocolate spotted! Keep going!";
          renderBoard();
        }, 700);
      } else {
        messageEl.textContent = "The dragon is getting closer...";
      }

      renderBoard();
    }

    document.addEventListener("keydown", (event) => {
      const key = event.key.toLowerCase();

      if (key === "arrowup" || key === "w") moveDragon(0, -1);
      else if (key === "arrowdown" || key === "s") moveDragon(0, 1);
      else if (key === "arrowleft" || key === "a") moveDragon(-1, 0);
      else if (key === "arrowright" || key === "d") moveDragon(1, 0);
    });

    updateStatus();
    renderBoard();
  </script>
</body>
</html># Game
