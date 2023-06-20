---
title: Have some fun!
layout: page
permalink: /game
---
<html>
<head>
  <title>2048</title>
  <style>
    .instruction {
    	text-align: center;
    	font-size: 14px;
    	color: #776e65;
    	font-family: Arial, sans-serif;
    }
    .game-container {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: auto;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 10px;
      background-color: #bbada0;
      padding: 10px;
      border-radius: 5px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }
    .cell {
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 24px;
      font-weight: bold;
      background-color: hsl(35, 29%, 85%);
      color: #776e65;
      border-radius: 5px;
      width: 80px;
      height: 80px;
    }  
    .game-button {
        background-color: hsl(35, 29%, 85%);
        color: #776e65;
        border: 4px solid hsl(35, 29%, 70%);
        padding: 10px 20px;
        border-radius: 5px;
        font-size: 20px;
        cursor: pointer;
    }
    .restart-container {
        display:grid;
        justify-content: center;
        align-items:start;
        margin-top: 20px;
    }
  </style>
</head>
<body>
  <div class="instruction">
    Press WASD or swipe the screen to start.  
    <br />
    Press R or click restart button to restart the game.
  </div>
  <div class="game-container">
    <div class="grid">
      <!-- 游戏方格 -->
      <div class="cell">0</div>
      <div class="cell">0</div>
      <div class="cell">0</div>
      <div class="cell">0</div>
      <div class="cell">0</div>
      <div class="cell">0</div>
      <div class="cell">0</div>
      <div class="cell">0</div>
      <div class="cell">0</div>
      <div class="cell">0</div>
      <div class="cell">0</div>
      <div class="cell">0</div>
      <div class="cell">0</div>
      <div class="cell">0</div>
      <div class="cell">0</div>
      <div class="cell">0</div>
    </div>
    <div class="restart-container">
        <button id="restart-button" class="game-button"><i>Restart Game</i></button>
    </div>
  </div>
  <footer>
    <p>
    <br />
    99% of this game is created by ChatGPT. 
    <br />
    Welcome to the new era of AI!   ❛‿˂̵✧
    </p>
  </footer>
  <script>
    // JavaScript 代码
    // 创建一个二维数组表示游戏方格
    const grid = [
      [0, 0, 0, 0],
      [0, 0, 0, 0],
      [0, 0, 0, 0],
      [0, 0, 0, 0]
    ];
    // 在 JavaScript 中获取按钮元素
    const restartButton = document.getElementById("restart-button");
    restartButton.addEventListener("click", restartGame);
    // 在页面加载完成后执行初始化操作
    document.addEventListener("DOMContentLoaded", () => {
      // 初始化游戏界面
      initializeGrid();
      // 监听键盘事件
      document.addEventListener("keydown", handleKeyPress);
      // 监听触摸事件
      document.addEventListener("touchstart", handleTouchStart, false);
      document.addEventListener("touchmove", handleTouchMove, false);
      document.addEventListener("touchend", handleTouchEnd, false);
      // 禁用页面滑动
      document.addEventListener('touchmove', function(event) {
        event.preventDefault();
      }, { passive: false });
    });
    // 初始化游戏界面
    function initializeGrid() {
      const gridElement = document.querySelector(".grid");
      gridElement.innerHTML = "";
      // 根据 grid 数组生成游戏方格
      for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[i].length; j++) {
          const cellElement = document.createElement("div");
          cellElement.classList.add("cell");
          cellElement.textContent = grid[i][j];
          gridElement.appendChild(cellElement);
        }
      }
    }
    // 重新开始游戏
    function restartGame(){
        //clear all grid
        for (let j = 0; j < grid[0].length; j++) {
            for (let i = 0; i < grid.length; i++) {
                    grid[i][j] = 0
            }
        }
        // 随机生成新的方块
        generateNewBlock();
        // 更新游戏界面
        updateGrid();
    }
    // 处理键盘按下事件
    function handleKeyPress(event) {
      if (event.key === "w" || event.key === "W") {
        moveUp();
      } else if (event.key === "s" || event.key === "S") {
        moveDown();
      } else if (event.key === "a" || event.key === "A") {
        moveLeft();
      } else if (event.key === "d" || event.key === "D") {
        moveRight();
      } else if (event.key === "r" || event.key === "R") {
        restartGame()
        return
      } else {
        return
      }
      // 随机生成新的方块
      generateNewBlock();
      // 更新游戏界面
      updateGrid();
      // 判断游戏是否胜利或失败
      checkGameOver();
    }
    let startX, startY;
    const touchThreshold = 50; // 滑动阈值，小于该值不触发移动操作
    // 触摸开始事件处理
    function handleTouchStart(event) {
      const touch = event.touches[0];
      startX = touch.clientX;
      startY = touch.clientY;
    }
    // 触摸移动事件处理
    function handleTouchMove(event) {
      event.preventDefault();
    }
    // 触摸结束事件处理
    function handleTouchEnd(event) {
      const touch = event.changedTouches[0];
      const endX = touch.clientX;
      const endY = touch.clientY;
      const deltaX = endX - startX;
      const deltaY = endY - startY;
      if (Math.abs(deltaX) < touchThreshold && Math.abs(deltaY) < touchThreshold) {
        // 滑动距离太小，忽略滑动操作
        return;
      }
      if (Math.abs(deltaX) > Math.abs(deltaY)) {
        if (deltaX > 0) {
          moveRight();
        } else {
          moveLeft();
        }
      } else {
        if (deltaY > 0) {
          moveDown();
        } else {
          moveUp();
        }
      }
      // 随机生成新的方块
      generateNewBlock();
      // 更新游戏界面
      updateGrid();
      // 判断游戏是否胜利或失败
      checkGameOver();
    }
    // 随机生成新的方块
    function generateNewBlock() {
      const emptyCells = [];
      for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[i].length; j++) {
          if (grid[i][j] === 0) {
            emptyCells.push({ row: i, col: j });
          }
        }
      }
      if (emptyCells.length > 0) {
        const randomIndex = Math.floor(Math.random() * emptyCells.length);
        const { row, col } = emptyCells[randomIndex];
        grid[row][col] = Math.random() < 0.9 ? 2 : 4;
      }
    }
    // 向上移动逻辑
    function moveUp() {
      for (let j = 0; j < grid[0].length; j++) {
        for (let i = 1; i < grid.length; i++) {
          if (grid[i][j] !== 0) {
            let k = i;
            while (k > 0 && grid[k - 1][j] === 0) {
              grid[k - 1][j] = grid[k][j];
              grid[k][j] = 0;
              k--;
            }
            if (k > 0 && grid[k - 1][j] === grid[k][j]) {
              grid[k - 1][j] *= 2;
              grid[k][j] = 0;
            }
          }
        }
      }
    }
    // 向下移动逻辑
    function moveDown() {
      for (let j = 0; j < grid[0].length; j++) {
        for (let i = grid.length - 2; i >= 0; i--) {
          if (grid[i][j] !== 0) {
            let k = i;
            while (k < grid.length - 1 && grid[k + 1][j] === 0) {
              grid[k + 1][j] = grid[k][j];
              grid[k][j] = 0;
              k++;
            }
            if (k < grid.length - 1 && grid[k + 1][j] === grid[k][j]) {
              grid[k + 1][j] *= 2;
              grid[k][j] = 0;
            }
          }
        }
      }
    }
    // 向左移动逻辑
    function moveLeft() {
      for (let i = 0; i < grid.length; i++) {
        for (let j = 1; j < grid[i].length; j++) {
          if (grid[i][j] !== 0) {
            let k = j;
            while (k > 0 && grid[i][k - 1] === 0) {
              grid[i][k - 1] = grid[i][k];
              grid[i][k] = 0;
              k--;
            }
            if (k > 0 && grid[i][k - 1] === grid[i][k]) {
              grid[i][k - 1] *= 2;
              grid[i][k] = 0;
            }
          }
        }
      }
    }
    // 向右移动逻辑
    function moveRight() {
      for (let i = 0; i < grid.length; i++) {
        for (let j = grid[i].length - 2; j >= 0; j--) {
          if (grid[i][j] !== 0) {
            let k = j;
            while (k < grid[i].length - 1 && grid[i][k + 1] === 0) {
              grid[i][k + 1] = grid[i][k];
              grid[i][k] = 0;
              k++;
            }
            if (k < grid[i].length - 1 && grid[i][k + 1] === grid[i][k]) {
              grid[i][k + 1] *= 2;
              grid[i][k] = 0;
            }
          }
        }
      }
    }
    // 在生成新方块时计算亮度
    function calculateLight(value) {
        // 计算饱和度的递增步长
        const step = Math.log2(2048);

        // 根据方块的值计算亮度
        return (Math.log2(value) / step) * 100;
    }
    // 更新游戏界面
    function updateGrid() {
      const gridElement = document.querySelector(".grid");
      // 移除所有子元素
      while (gridElement.firstChild) {
        gridElement.firstChild.remove();
      }
      // 更新游戏方格
      for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[i].length; j++) {
          const cellElement = document.createElement("div");
          cellElement.classList.add("cell");
          cellElement.textContent = grid[i][j];
          const light = calculateLight(grid[i][j]);
          cellElement.style.backgroundColor = `hsl(39, 29%, ${80-light*0.25}%)`; // change the light between 55%-80%
          gridElement.appendChild(cellElement);
        }
      }
    }
    // 判断游戏是否胜利或失败
    function checkGameOver() {
      // 检查是否有格子的值等于 2048，如果有，则游戏胜利
      for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[i].length; j++) {
          if (grid[i][j] === 2048) {
            console.log("You are the best! (ᕑᗢᓫ∗)˒");
            return;
          }
        }
      }
      // 检查是否所有格子都被填满，如果是且无法再进行移动操作，则游戏失败
      let isFull = true;
      for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[i].length; j++) {
          if (grid[i][j] === 0) {
            isFull = false;
            break;
          }
        }
        if (!isFull) {
          break;
        }
      }
      // 如果所有格子都被填满，检查是否无法再进行移动操作
      if (isFull) {
        let canMove = false;
        // 检查垂直方向是否还能进行合并
        for (let j = 0; j < grid[0].length; j++) {
          for (let i = 0; i < grid.length - 1; i++) {
            if (grid[i][j] === grid[i + 1][j]) {
              canMove = true;
              break;
            }
          }
          if (canMove) {
            break;
          }
        }
        // 检查水平方向是否还能进行合并
        if (!canMove) {
          for (let i = 0; i < grid.length; i++) {
            for (let j = 0; j < grid[i].length - 1; j++) {
              if (grid[i][j] === grid[i][j + 1]) {
                canMove = true;
                break;
              }
            }
            if (canMove) {
              break;
            }
          }
        }
        if (!canMove) {
            alert("Gameover •ࡇ• press R or click the button to try again");
        }
      }
    }
  </script>
</body>
</html>
