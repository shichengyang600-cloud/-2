<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <!-- 防止手機縮放與雙擊延遲 -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
  <title>療癒俄羅斯方塊</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@600;700&family=Zen+Maru+Gothic:wght@500;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg-gradient: linear-gradient(145deg, #f7f3ee 0%, #edf3f7 50%, #f4eff6 100%);
      --card-bg: rgba(255, 255, 255, 0.78);
      --card-border: rgba(255, 255, 255, 0.9);
      --text-main: #334155;
      --text-muted: #798a9d;
      --accent-deep: #476882;
      --accent-soft: #eaf1f7;
      --btn-active: #dce7f0;
      --shadow-soft: 0 12px 30px rgba(118, 137, 161, 0.12);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Zen Maru Gothic', 'Plus Jakarta Sans', sans-serif;
      -webkit-tap-highlight-color: transparent;
      user-select: none;
      -webkit-user-select: none;
    }

    body {
      background: var(--bg-gradient);
      color: var(--text-main);
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      min-height: 100dvh;
      overflow-x: hidden;
      touch-action: manipulation;
    }

    .app-container {
      width: 100%;
      max-width: 820px;
      padding: 16px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 14px;
    }

    /* 頂部標題 */
    header {
      text-align: center;
    }

    header h1 {
      font-size: 1.5rem;
      letter-spacing: 2px;
      color: #273849;
    }

    header p {
      font-size: 0.85rem;
      color: var(--text-muted);
      margin-top: 4px;
    }

    /* 遊戲主區塊卡片 */
    .game-board-card {
      background: var(--card-bg);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      border: 1px solid var(--card-border);
      box-shadow: var(--shadow-soft);
      border-radius: 24px;
      padding: 18px;
      display: flex;
      gap: 20px;
      align-items: center;
      justify-content: center;
      width: 100%;
      max-width: 580px;
    }

    .canvas-wrapper {
      position: relative;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    #tetris {
      background: #fdfefe;
      border: 3px solid #ffffff;
      border-radius: 18px;
      box-shadow: inset 0 2px 10px rgba(0,0,0,0.02), 0 8px 20px rgba(118, 137, 161, 0.12);
      display: block;
      max-width: 100%;
      height: auto;
    }

    /* 側邊資訊欄 */
    .side-info {
      display: flex;
      flex-direction: column;
      gap: 12px;
      width: 140px;
    }

    .info-box {
      background: rgba(255, 255, 255, 0.65);
      border: 1px solid rgba(255, 255, 255, 0.85);
      border-radius: 16px;
      padding: 12px;
      text-align: center;
      box-shadow: 0 4px 10px rgba(0,0,0,0.015);
    }

    .info-label {
      font-size: 0.75rem;
      font-weight: 700;
      color: var(--text-muted);
      letter-spacing: 1px;
    }

    .info-value {
      font-family: 'Plus Jakarta Sans', sans-serif;
      font-size: 1.6rem;
      font-weight: 700;
      color: var(--accent-deep);
      margin-top: 2px;
    }

    #next {
      display: block;
      margin: 4px auto 0;
      border-radius: 10px;
    }

    .aux-controls {
      display: flex;
      flex-direction: column;
      gap: 8px;
    }

    .aux-btn {
      background: #ffffff;
      border: 1px solid rgba(215, 225, 235, 0.8);
      color: var(--text-main);
      border-radius: 12px;
      padding: 8px 10px;
      font-size: 0.82rem;
      font-weight: 700;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 4px;
      transition: all 0.15s ease;
      box-shadow: 0 2px 4px rgba(0,0,0,0.02);
    }

    .aux-btn:active {
      transform: scale(0.96);
      background: var(--btn-active);
    }

    .aux-btn.active {
      background: var(--accent-deep);
      color: #fff;
    }

    /* 手機虛擬手把 */
    .mobile-controls {
      display: grid;
      grid-template-columns: repeat(3, 1fr) 1.2fr;
      gap: 8px;
      width: 100%;
      max-width: 440px;
      padding: 4px;
    }

    .d-pad {
      grid-column: span 3;
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 6px;
    }

    .act-pad {
      display: flex;
      flex-direction: column;
      gap: 6px;
    }

    .touch-btn {
      background: #ffffff;
      border: 1px solid rgba(220, 230, 240, 0.9);
      border-radius: 16px;
      height: 52px;
      font-size: 1.2rem;
      font-weight: 700;
      color: var(--accent-deep);
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      box-shadow: 0 4px 10px rgba(118, 137, 161, 0.08);
      touch-action: manipulation;
    }

    .touch-btn:active {
      background: var(--accent-soft);
      transform: scale(0.94);
    }

    .touch-btn.primary {
      background: #88a4bc;
      color: #ffffff;
      border-color: #88a4bc;
    }

    .touch-btn.primary:active {
      background: var(--accent-deep);
    }

    .key-hints-desktop {
      font-size: 0.8rem;
      color: var(--text-muted);
      text-align: center;
    }

    /* 手機畫面適配 */
    @media (max-width: 480px) {
      .game-board-card {
        padding: 12px;
        gap: 12px;
        border-radius: 20px;
      }

      #tetris {
        width: 180px;
        height: 300px;
      }

      .side-info {
        width: 100px;
        gap: 8px;
      }

      .info-value {
        font-size: 1.3rem;
      }

      .key-hints-desktop {
        display: none;
      }
    }
  </style>
</head>
<body>

<div class="app-container">
  <header>
    <h1>🌱 療癒俄羅斯方塊</h1>
    <p>深呼吸 · 安放每一份平靜</p>
  </header>

  <div class="game-board-card">
    <div class="canvas-wrapper">
      <canvas id="tetris" width="240" height="400"></canvas>
    </div>

    <div class="side-info">
      <div class="info-box">
        <div class="info-label">分數</div>
        <div id="score" class="info-value">0</div>
      </div>

      <div class="info-box">
        <div class="info-label">下一塊</div>
        <canvas id="next" width="70" height="70"></canvas>
      </div>

      <div class="aux-controls">
        <button id="btn-undo" class="aux-btn">↩ 倒退</button>
        <button id="btn-freeze" class="aux-btn">☁️ 懸浮</button>
        <button id="btn-pause" class="aux-btn">⏸ 暫停</button>
      </div>
    </div>
  </div>

  <!-- 手機虛擬按鍵 -->
  <div class="mobile-controls">
    <div class="d-pad">
      <div></div>
      <button class="touch-btn" id="ctrl-up">↑</button>
      <div></div>
      <button class="touch-btn" id="ctrl-left">←</button>
      <button class="touch-btn" id="ctrl-down">↓</button>
      <button class="touch-btn" id="ctrl-right">→</button>
    </div>
    <div class="act-pad">
      <button class="touch-btn primary" id="ctrl-drop">落底</button>
      <button class="touch-btn" id="ctrl-rotate">旋轉</button>
    </div>
  </div>

  <div class="key-hints-desktop">
    鍵盤操作：← → 移動 ｜ ↑ 旋轉 ｜ ↓ 加速 ｜ Space 落底 ｜ Z 回上一步
  </div>
</div>

<script>
  const canvas = document.getElementById('tetris');
  const context = canvas.getContext('2d');
  const nextCanvas = document.getElementById('next');
  const nextContext = nextCanvas.getContext('2d');

  context.scale(20, 20);
  nextContext.scale(14, 14);

  const PALETTE = [
    null,
    { main: '#8ca9be', light: '#b9d2e3' }, // I
    { main: '#9bbcd1', light: '#cae2f0' }, // J
    { main: '#d5b29c', light: '#efd5c5' }, // L
    { main: '#ecc97b', light: '#f7e3ad' }, // O
    { main: '#8ec5a6', light: '#c0e5d0' }, // S
    { main: '#bfa7d5', light: '#e1d2f0' }, // T
    { main: '#dea0a0', light: '#f3c7c7' }, // Z
  ];

  function createMatrix(w, h) {
    const matrix = [];
    while (h--) matrix.push(new Array(w).fill(0));
    return matrix;
  }

  function createPiece(type) {
    if (type === 'I') return [[0,1,0,0],[0,1,0,0],[0,1,0,0],[0,1,0,0]];
    if (type === 'L') return [[0,2,0],[0,2,0],[0,2,2]];
    if (type === 'J') return [[0,3,0],[0,3,0],[3,3,0]];
    if (type === 'O') return [[4,4],[4,4]];
    if (type === 'Z') return [[5,5,0],[0,5,5],[0,0,0]];
    if (type === 'S') return [[0,6,6],[6,6,0],[0,0,0]];
    if (type === 'T') return [[0,7,0],[7,7,7],[0,0,0]];
  }

  const arena = createMatrix(12, 20);
  const player = {
    pos: { x: 0, y: 0 },
    matrix: null,
    next: null,
    score: 0,
  };

  let isPaused = false;
  let isFrozen = false;
  let dropInterval = 850;
  let dropCounter = 0;
  let lastTime = 0;
  let historyStack = [];

  function saveState() {
    historyStack.push(JSON.stringify({
      arena: arena.map(row => [...row]),
      playerPos: { ...player.pos },
      playerMatrix: player.matrix ? player.matrix.map(row => [...row]) : null,
      playerNext: player.next,
      score: player.score
    }));
    if (historyStack.length > 25) historyStack.shift();
  }

  function undo() {
    if (!historyStack.length) return;
    const prev = JSON.parse(historyStack.pop());
    for (let y = 0; y < arena.length; ++y) {
      for (let x = 0; x < arena[y].length; ++x) {
        arena[y][x] = prev.arena[y][x];
      }
    }
    player.pos = prev.playerPos;
    player.matrix = prev.playerMatrix;
    player.next = prev.playerNext;
    player.score = prev.score;
    document.getElementById('score').innerText = player.score;
    drawNext();
  }

  function collide(arena, player) {
    const [m, o] = [player.matrix, player.pos];
    for (let y = 0; y < m.length; ++y) {
      for (let x = 0; x < m[y].length; ++x) {
        if (m[y][x] !== 0 && (arena[y + o.y] && arena[y + o.y][x + o.x]) !== 0) {
          return true;
        }
      }
    }
    return false;
  }

  function arenaSweep() {
    let rowCount = 1;
    outer: for (let y = arena.length - 1; y >= 0; --y) {
      for (let x = 0; x < arena[y].length; ++x) {
        if (arena[y][x] === 0) continue outer;
      }
      const row = arena.splice(y, 1)[0].fill(0);
      arena.unshift(row);
      ++y;
      player.score += rowCount * 10;
      rowCount *= 2;
    }
    document.getElementById('score').innerText = player.score;
  }

  function merge(arena, player) {
    player.matrix.forEach((row, y) => {
      row.forEach((value, x) => {
        if (value !== 0) arena[y + player.pos.y][x + player.pos.x] = value;
      });
    });
  }

  function drawRoundedBlock(ctx, x, y, colorIdx, isGhost = false) {
    const p = PALETTE[colorIdx];
    const pad = 0.08;
    const size = 1 - pad * 2;
    const radius = 0.22;

    ctx.save();
    ctx.translate(x + pad, y + pad);
    ctx.beginPath();
    ctx.roundRect(0, 0, size, size, radius);

    if (isGhost) {
      ctx.strokeStyle = p.main;
      ctx.lineWidth = 0.08;
      ctx.setLineDash([0.15, 0.1]);
      ctx.stroke();
      ctx.fillStyle = p.light + '28';
      ctx.fill();
      ctx.restore();
      return;
    }

    ctx.fillStyle = p.main;
    ctx.fill();

    ctx.beginPath();
    ctx.roundRect(0.06, 0.06, size - 0.12, (size - 0.12) * 0.45, [radius * 0.7, radius * 0.7, 0, 0]);
    ctx.fillStyle = 'rgba(255, 255, 255, 0.4)';
    ctx.fill();

    ctx.restore();
  }

  function getGhostY() {
    if (!player.matrix) return player.pos.y;
    const ghost = { pos: { x: player.pos.x, y: player.pos.y }, matrix: player.matrix };
    while (!collide(arena, ghost)) ghost.pos.y++;
    return ghost.pos.y - 1;
  }

  function draw() {
    context.fillStyle = '#fbfcff';
    context.fillRect(0, 0, canvas.width, canvas.height);

    context.strokeStyle = 'rgba(210, 225, 235, 0.35)';
    context.lineWidth = 0.03;
    for (let x = 0; x <= 12; x++) {
      context.beginPath(); context.moveTo(x, 0); context.lineTo(x, 20); context.stroke();
    }
    for (let y = 0; y <= 20; y++) {
      context.beginPath(); context.moveTo(0, y); context.lineTo(12, y); context.stroke();
    }

    arena.forEach((row, y) => {
      row.forEach((value, x) => {
        if (value !== 0) drawRoundedBlock(context, x, y, value);
      });
    });

    if (player.matrix) {
      const ghostY = getGhostY();
      if (ghostY !== player.pos.y) {
        player.matrix.forEach((row, y) => {
          row.forEach((val, x) => {
            if (val !== 0) drawRoundedBlock(context, x + player.pos.x, y + ghostY, val, true);
          });
        });
      }

      player.matrix.forEach((row, y) => {
        row.forEach((value, x) => {
          if (value !== 0) drawRoundedBlock(context, x + player.pos.x, y + player.pos.y, value);
        });
      });
    }
  }

  function drawNext() {
    nextContext.clearRect(0, 0, nextCanvas.width, nextCanvas.height);
    if (!player.next) return;
    const piece = createPiece(player.next);
    const offsetX = (5 - piece[0].length) / 2;
    const offsetY = (5 - piece.length) / 2;
    piece.forEach((row, y) => {
      row.forEach((val, x) => {
        if (val !== 0) drawRoundedBlock(nextContext, x + offsetX, y + offsetY, val);
      });
    });
  }

  function playerDrop() {
    if (isPaused) return;
    saveState();
    player.pos.y++;
    if (collide(arena, player)) {
      player.pos.y--;
      merge(arena, player);
      playerReset();
      arenaSweep();
    }
    dropCounter = 0;
  }

  function playerHardDrop() {
    if (isPaused) return;
    saveState();
    player.pos.y = getGhostY();
    merge(arena, player);
    playerReset();
    arenaSweep();
    dropCounter = 0;
  }

  function playerMove(dir) {
    if (isPaused) return;
    saveState();
    player.pos.x += dir;
    if (collide(arena, player)) player.pos.x -= dir;
  }

  function playerRotate(dir = 1) {
    if (isPaused) return;
    saveState();
    const pos = player.pos.x;
    let offset = 1;
    rotate(player.matrix, dir);
    while (collide(arena, player)) {
      player.pos.x += offset;
      offset = -(offset + (offset > 0 ? 1 : -1));
      if (offset > player.matrix[0].length) {
        rotate(player.matrix, -dir);
        player.pos.x = pos;
        return;
      }
    }
  }

  function rotate(matrix, dir) {
    for (let y = 0; y < matrix.length; ++y) {
      for (let x = 0; x < y; ++x) {
        [matrix[x][y], matrix[y][x]] = [matrix[y][x], matrix[x][y]];
      }
    }
    if (dir > 0) matrix.forEach(row => row.reverse());
    else matrix.reverse();
  }

  function playerReset() {
    const pieces = 'ILJOTSZ';
    if (!player.next) player.next = pieces[pieces.length * Math.random() | 0];
    player.matrix = createPiece(player.next);
    player.next = pieces[pieces.length * Math.random() | 0];
    player.pos.y = 0;
    player.pos.x = (arena[0].length / 2 | 0) - (player.matrix[0].length / 2 | 0);
    drawNext();

    if (collide(arena, player)) {
      arena.forEach(row => row.fill(0));
      player.score = 0;
      document.getElementById('score').innerText = 0;
    }
  }

  function update(time = 0) {
    const deltaTime = time - lastTime;
    lastTime = time;

    if (!isPaused && !isFrozen) {
      dropCounter += deltaTime;
      if (dropCounter > dropInterval) playerDrop();
    }
    draw();
    requestAnimationFrame(update);
  }

  document.addEventListener('keydown', e => {
    if (e.keyCode === 37) playerMove(-1);
    else if (e.keyCode === 39) playerMove(1);
    else if (e.keyCode === 40) playerDrop();
    else if (e.keyCode === 38) playerRotate(1);
    else if (e.keyCode === 32) { e.preventDefault(); playerHardDrop(); }
    else if (e.keyCode === 90 || e.keyCode === 122) undo();
    else if (e.keyCode === 80 || e.keyCode === 112) togglePause();
    else if (e.keyCode === 70 || e.keyCode === 102) toggleFreeze();
  });

  function bindTouch(elementId, action) {
    const el = document.getElementById(elementId);
    let isTouch = false;
    el.addEventListener('touchstart', e => {
      e.preventDefault();
      isTouch = true;
      action();
    }, { passive: false });
    el.addEventListener('click', () => {
      if (!isTouch) action();
      isTouch = false;
    });
  }

  bindTouch('ctrl-left', () => playerMove(-1));
  bindTouch('ctrl-right', () => playerMove(1));
  bindTouch('ctrl-up', () => playerRotate(1));
  bindTouch('ctrl-rotate', () => playerRotate(1));
  bindTouch('ctrl-down', () => playerDrop());
  bindTouch('ctrl-drop', () => playerHardDrop());

  const pauseBtn = document.getElementById('btn-pause');
  const freezeBtn = document.getElementById('btn-freeze');

  function togglePause() {
    isPaused = !isPaused;
    pauseBtn.classList.toggle('active', isPaused);
    pauseBtn.innerText = isPaused ? '▶ 繼續' : '⏸ 暫停';
  }

  function toggleFreeze() {
    isFrozen = !isFrozen;
    freezeBtn.classList.toggle('active', isFrozen);
  }

  bindTouch('btn-pause', togglePause);
  bindTouch('btn-freeze', toggleFreeze);
  bindTouch('btn-undo', undo);

  playerReset();
  update();
</script>
</body>
</html>
