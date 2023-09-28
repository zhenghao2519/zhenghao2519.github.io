---
title: Play 2048!
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
      background-color: #faf8ef;
      margin-top: 20px;
      margin-bottom: 10px;
    }

    .score {
      text-align: center;
      font-weight: bold;
      font-size: 20px;
      color: #776e65;
      font-family: Arial, sans-serif;
      background-color: #faf8ef;
    }
    
    .game-container {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: auto;
      background-color: #faf8ef;
      border-radius: 50px;
    }
    
    .grid {
      zoom: 90%;
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 10px;
      background-color: #bbada0;
      padding: 10px;
      border-radius: 5px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
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
      border-radius: 15px;
      font-size: 20px;
      cursor: pointer;
    }
    
    .restart-container {
      display: grid;
      justify-content: center;
      align-items: start;
      margin-top: 20px;
      margin-bottom: 10px;
    }
    
    /* 排行榜容器 */
    .rankings-container {
      text-align: center;
      /* 左对齐 */
      font-family: Arial, sans-serif;
      background-color: #faf8ef;
      margin-top: 20px;
      margin-bottom: 10px;
    }
    
    /* 排行榜标题样式 */
    .rankings-title {
      font-size: 18px;
      font-weight: bold;
      color: #776e65;
      margin-bottom: 10px;
    }
    
    /* 排行榜条目样式 */
    .rankings-item {
      font-size: 16px;
      color: #333;
      margin-bottom: 8px;
      display: flex;
      /* 使用 flex 布局 */
      justify-content: space-between;
      /* 左右对齐 */
      align-items: center;
      /* 垂直居中对齐 */
    }
    
    /* 排行榜玩家名字 */
    .rankings-player {
      font-weight: bold;
      margin-right: 40px;
    }
    
    /* 排行榜分数 */
    .rankings-score {
      font-weight: bold;
      color: #e65100;
      /* 橙色字体 */
    }
  </style>
</head>

<body>
  <div class="game-container" id="game">
    <div class="instruction">
      Press WASD or swipe the screen to start.
      <br />
      Press R or click restart button to restart the game.
      <br />
      Detailed rules of 2048 can be found in <a href="https://en.wikipedia.org/wiki/2048_(video_game)#Gameplay"
        style="color: #776e65;"><i><b>Rules of 2048</b></i></a>
    </div>
    Score: <div class="score" id="score">0</div>
    <div class="grid" id="grid">
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
    <div class="rankings-container" id="ranking">
      <div class="rankings-title">Highest Scores</div>
      <div class="rankings-item">
        <div class="rankings-player">player1</div>
        <div class="rankings-score">1000</div>
      </div>
    </div>
  </div>
  <footer>
    <p>
      <br />
      Realtime Ranking is supported by Firebase.
      <br />
      Have fun! ❛‿˂̵✧
    </p>
  </footer>
  <script type="module">
    // Import the functions you need from t)he SDKs you need
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.4.0/firebase-app.js";
    import { getAnalytics } from "https://www.gstatic.com/firebasejs/10.4.0/firebase-analytics.js";
    import { getDatabase, ref, set, onValue } from "https://www.gstatic.com/firebasejs/10.4.0/firebase-database.js";
    // TODO: Add SDKs for Firebase products that you want to use
    // https://firebase.google.com/docs/web/setup#available-libraries

    // Your web app's Firebase configuration
    // For Firebase JS SDK v7.20.0 and later, measurementId is optional
    const firebaseConfig = {
      apiKey: "AIzaSyAUKJ2VNZ5PzFcWqZAM7MPYgjkn-6-NW5o",
      authDomain: "persenal-web-2048.firebaseapp.com",
      projectId: "persenal-web-2048",
      storageBucket: "persenal-web-2048.appspot.com",
      messagingSenderId: "898743704734",
      appId: "1:898743704734:web:d6b048495a8ce80eaaef8f",
      measurementId: "G-MM7EJS4YSC",
      databaseURL: "https://persenal-web-2048-default-rtdb.europe-west1.firebasedatabase.app/",
    };
    
    // Initialize Firebase
    const app = initializeApp(firebaseConfig);
    const analytics = getAnalytics(app);
    const database = getDatabase(app);
    
    function writeUserScore(name, score) {
      set(ref(database, 'ranking/' + name), {
        score: parseInt(score)
      });
    }
    window.writeUserScore = writeUserScore;
    
    const rankingElement = document.getElementById('ranking');
    const ranking = ref(database, 'ranking/');
    onValue(ranking, (snapshot) => {
      const ranktable = snapshot.val();
      const dataArray = Object.entries(ranktable).map(([key, value]) => ({ name: key, score: value.score }));
      // 根据分数属性进行排序（从高到低）
      const sortedData = dataArray.sort((a, b) => b.score - a.score);
    
      updateRanking(sortedData);
    });
    
    function updateRanking(sortedData) {
      while (rankingElement.children[1]) {
        rankingElement.children[1].remove();
      }
      for (let i = 0; i < 3; i++) {
        const rankingItemElement = document.createElement("div");
        rankingItemElement.classList.add("rankings-item");
    
        const rankingPlayerElement = document.createElement("div");
        rankingPlayerElement.classList.add("rankings-player");
        rankingPlayerElement.textContent = sortedData[i].name;
    
        const rankingScoreElement = document.createElement("div");
        rankingScoreElement.classList.add("rankings-score");
        rankingScoreElement.textContent = sortedData[i].score;
    
        rankingItemElement.appendChild(rankingPlayerElement);
        rankingItemElement.appendChild(rankingScoreElement);
        rankingElement.appendChild(rankingItemElement);
      }
    
    }

  </script>
  <script type="module">
    // JavaScript 代码 for 2048 gamelogic
    // 创建一个二维数组表示游戏方格
    const grid = [
      [0, 0, 0, 0],
      [0, 0, 0, 0],
      [0, 0, 0, 0],
      [0, 0, 0, 0]
    ];
    // 获取游戏容器元素
    const gameElement = document.getElementById('grid');
    // 获取游戏分数元素
    const scoreElement = document.getElementById('score');
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
      // 禁用网页的默认滚动行为
      document.addEventListener('touchmove', function (event) {
        if (gameElement.contains(event.target)) {
          // 不在游戏界面内的滑动操作，允许页面滚动
          event.preventDefault();
        }
      }, { passive: false });
      document.addEventListener("touchend", handleTouchEnd, false);
    });
    // 初始化游戏界面
    function initializeGrid() {
      gameElement.innerHTML = "";
      // 根据 grid 数组生成游戏方格
      for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[i].length; j++) {
          const cellElement = document.createElement("div");
          cellElement.classList.add("cell");
          cellElement.textContent = grid[i][j];
          gameElement.appendChild(cellElement);
        }
      }
    }
    // 重新开始游戏
    function restartGame() {
      //clear score
      scoreElement.textContent = 0;
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
        restartGame();
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
    let startX, startY, valid = false;
    const touchThreshold = 50; // 滑动阈值，小于该值不触发移动操作
    // 触摸开始事件处理
    function handleTouchStart(event) {
      const touch = event.touches[0];
      startX = touch.clientX;
      startY = touch.clientY;
      if (gameElement.contains(event.target)) {
        valid = true;
      } else {
        // 不在游戏界面内的滑动操作，允许页面滚动, 不允许逻辑运行
        valid = false;
      }
    }
    // 触摸结束事件处理
    function handleTouchEnd(event) {
      const touch = event.changedTouches[0];
      const endX = touch.clientX;
      const endY = touch.clientY;
      const deltaX = endX - startX;
      const deltaY = endY - startY;
      if (valid == false) {
        // touch outside of game field
        return;
      }
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
              updateScore(grid[k - 1][j]);
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
              updateScore(grid[k + 1][j]);
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
              updateScore(grid[i][k - 1]);
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
              updateScore(grid[i][k + 1]);
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
      //const gridElement = document.querySelector(".grid");
      // 移除所有子元素
      while (gameElement.firstChild) {
        gameElement.firstChild.remove();
      }
      // 更新游戏方格
      for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[i].length; j++) {
          const cellElement = document.createElement("div");
          cellElement.classList.add("cell");
          cellElement.textContent = grid[i][j];
          const light = calculateLight(grid[i][j]);
          cellElement.style.backgroundColor = `hsl(39, 29%, ${80 - light * 0.25}%)`; // change the light between 55%-80%
          gameElement.appendChild(cellElement);
        }
      }
    }
    // 判断游戏是否胜利或失败
    function checkGameOver() {
      // 检查是否有格子的值等于 2048，如果有，则游戏胜利
      for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[i].length; j++) {
          if (grid[i][j] === 2048) {
            alert("You are the best! (ᕑᗢᓫ∗)˒");
            const name = prompt("Enter your name to upload your score：");
            if (name != null) {
              window.writeUserScore(name, scoreElement.textContent);
            }
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
          const name = prompt("Enter your name to upload your score：");
          if (name != null) {
            window.writeUserScore(name, scoreElement.textContent);
          }
        }
      }
    }
    //更新分数
    function updateScore(addScore) {
      scoreElement.textContent = parseInt(scoreElement.textContent) + addScore;
    }


  </script>


</body>

</html>