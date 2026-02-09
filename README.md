# malliao
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>马里奥冒险</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            touch-action: manipulation;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            font-family: 'Arial Rounded MT Bold', 'Arial', sans-serif;
            background: linear-gradient(to bottom, #87CEEB, #E0F7FF);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            overflow: hidden;
            color: #333;
            padding: 10px;
        }
        
        .container {
            display: flex;
            flex-direction: column;
            align-items: center;
            max-width: 1000px;
            width: 100%;
        }
        
        header {
            text-align: center;
            margin-bottom: 15px;
            width: 100%;
        }
        
        h1 {
            color: #E52521;
            text-shadow: 3px 3px 0 #FFD90F, 6px 6px 0 rgba(0, 0, 0, 0.2);
            font-size: clamp(28px, 6vw, 48px);
            margin-bottom: 5px;
            letter-spacing: 1px;
        }
        
        .subtitle {
            color: #1E7F1E;
            font-size: clamp(16px, 3vw, 20px);
            margin-bottom: 10px;
            font-weight: bold;
        }
        
        .game-container {
            position: relative;
            width: 100%;
            max-width: 800px;
            margin-bottom: 20px;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            background-color: #000;
        }
        
        canvas {
            display: block;
            width: 100%;
            height: auto;
            background-color: #000;
        }
        
        .stats {
            display: flex;
            justify-content: space-between;
            width: 100%;
            max-width: 800px;
            background-color: rgba(0, 0, 0, 0.8);
            color: white;
            padding: 12px 20px;
            border-radius: 10px;
            margin-bottom: 15px;
            font-size: clamp(16px, 3vw, 20px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }
        
        .stat-item {
            display: flex;
            align-items: center;
        }
        
        .stat-icon {
            margin-right: 8px;
            font-size: 1.5em;
        }
        
        .controls {
            display: flex;
            flex-direction: column;
            align-items: center;
            width: 100%;
            max-width: 800px;
            margin-top: 10px;
        }
        
        .keyboard-controls {
            color: #444;
            margin-bottom: 15px;
            text-align: center;
            font-size: clamp(14px, 2.5vw, 16px);
        }
        
        .mobile-controls {
            display: none;
            width: 100%;
            justify-content: space-between;
            margin-top: 10px;
        }
        
        .control-btn {
            background-color: #FFD90F;
            border: none;
            border-radius: 50%;
            width: 70px;
            height: 70px;
            font-size: 24px;
            color: #E52521;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 6px 0 #B89500, 0 8px 10px rgba(0, 0, 0, 0.3);
            user-select: none;
            touch-action: manipulation;
            margin: 0 10px;
        }
        
        .control-btn:active {
            transform: translateY(4px);
            box-shadow: 0 2px 0 #B89500, 0 4px 6px rgba(0, 0, 0, 0.3);
        }
        
        .dpad {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            grid-template-rows: repeat(3, 1fr);
            gap: 5px;
        }
        
        .dpad-btn {
            background-color: #5A5A5A;
            border: none;
            color: white;
            font-size: 20px;
            width: 50px;
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 8px;
            box-shadow: 0 4px 0 #333, 0 6px 8px rgba(0, 0, 0, 0.3);
        }
        
        .dpad-btn:active {
            transform: translateY(4px);
            box-shadow: 0 0 0 #333, 0 2px 4px rgba(0, 0, 0, 0.3);
        }
        
        .dpad-up {
            grid-column: 2;
            grid-row: 1;
        }
        
        .dpad-left {
            grid-column: 1;
            grid-row: 2;
        }
        
        .dpad-right {
            grid-column: 3;
            grid-row: 2;
        }
        
        .jump-btn {
            background-color: #E52521;
            color: white;
            font-weight: bold;
        }
        
        .instructions {
            background-color: rgba(255, 255, 255, 0.9);
            border-radius: 10px;
            padding: 15px;
            margin-top: 20px;
            width: 100%;
            max-width: 800px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }
        
        .instructions h3 {
            color: #1E7F1E;
            margin-bottom: 10px;
            font-size: clamp(18px, 4vw, 22px);
        }
        
        .instructions p {
            margin-bottom: 8px;
            line-height: 1.5;
            font-size: clamp(14px, 2.5vw, 16px);
        }
        
        .instructions ul {
            margin-left: 20px;
            margin-bottom: 10px;
        }
        
        .instructions li {
            margin-bottom: 5px;
        }
        
        .game-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            background-color: rgba(0, 0, 0, 0.85);
            color: white;
            z-index: 10;
            padding: 20px;
            text-align: center;
        }
        
        .game-overlay h2 {
            color: #FFD90F;
            font-size: clamp(32px, 7vw, 48px);
            margin-bottom: 15px;
            text-shadow: 3px 3px 0 #E52521;
        }
        
        .game-overlay p {
            font-size: clamp(18px, 4vw, 24px);
            margin-bottom: 20px;
            max-width: 600px;
        }
        
        .btn {
            background-color: #E52521;
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: clamp(18px, 4vw, 22px);
            border-radius: 50px;
            cursor: pointer;
            font-weight: bold;
            margin-top: 10px;
            box-shadow: 0 6px 0 #A01815, 0 8px 15px rgba(0, 0, 0, 0.3);
            transition: all 0.1s;
        }
        
        .btn:active {
            transform: translateY(4px);
            box-shadow: 0 2px 0 #A01815, 0 4px 8px rgba(0, 0, 0, 0.3);
        }
        
        .hidden {
            display: none !important;
        }
        
        @media (max-width: 768px) {
            .mobile-controls {
                display: flex;
            }
            
            .keyboard-controls {
                display: none;
            }
            
            .control-btn {
                width: 80px;
                height: 80px;
                font-size: 28px;
            }
            
            .stats {
                padding: 10px 15px;
            }
        }
        
        @media (max-width: 480px) {
            .control-btn {
                width: 70px;
                height: 70px;
                font-size: 24px;
                margin: 0 5px;
            }
            
            .dpad-btn {
                width: 45px;
                height: 45px;
            }
        }
        
        .footer {
            margin-top: 20px;
            color: #555;
            text-align: center;
            font-size: 14px;
            padding: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>马里奥冒险</h1>
            <div class="subtitle">收集金币，躲避敌人，到达终点！</div>
        </header>
        
        <div class="stats">
            <div class="stat-item">
                <span class="stat-icon">⭐</span>
                <span>关卡: <span id="level">1</span></span>
            </div>
            <div class="stat-item">
                <span class="stat-icon">💰</span>
                <span>金币: <span id="coins">0</span></span>
            </div>
            <div class="stat-item">
                <span class="stat-icon">❤️</span>
                <span>生命: <span id="lives">3</span></span>
            </div>
            <div class="stat-item">
                <span class="stat-icon">⏱️</span>
                <span>时间: <span id="time">300</span></span>
            </div>
        </div>
        
        <div class="game-container">
            <canvas id="gameCanvas"></canvas>
            
            <div id="startScreen" class="game-overlay">
                <h2>马里奥冒险</h2>
                <p>使用方向键移动，空格键跳跃。在手机上使用屏幕按钮控制。</p>
                <p>收集所有金币，躲避敌人，到达终点旗杆！</p>
                <button id="startBtn" class="btn">开始游戏</button>
            </div>
            
            <div id="gameOverScreen" class="game-overlay hidden">
                <h2>游戏结束</h2>
                <p id="gameOverMessage">你被敌人击败了！</p>
                <button id="restartBtn" class="btn">重新开始</button>
            </div>
            
            <div id="levelCompleteScreen" class="game-overlay hidden">
                <h2>关卡完成！</h2>
                <p id="levelCompleteMessage">你收集了 <span id="finalCoins">0</span> 枚金币</p>
                <button id="nextLevelBtn" class="btn">下一关</button>
            </div>
        </div>
        
        <div class="controls">
            <div class="keyboard-controls">
                键盘控制: 方向键移动, 空格键跳跃
            </div>
            
            <div class="mobile-controls">
                <div class="dpad">
                    <button class="dpad-btn dpad-up" id="upBtn">↑</button>
                    <button class="dpad-btn dpad-left" id="leftBtn">←</button>
                    <button class="dpad-btn dpad-right" id="rightBtn">→</button>
                </div>
                
                <button class="control-btn jump-btn" id="jumpBtn">跳</button>
            </div>
        </div>
        
        <div class="instructions">
            <h3>游戏说明</h3>
            <ul>
                <li><strong>目标：</strong>收集所有金币并到达终点旗杆</li>
                <li><strong>控制：</strong>电脑使用键盘方向键和空格键，手机使用屏幕按钮</li>
                <li><strong>敌人：</strong>碰到敌人会失去一条生命，踩敌人头顶可以消灭它们</li>
                <li><strong>金币：</strong>收集金币可以增加分数</li>
                <li><strong>时间：</strong>你只有300秒时间完成关卡</li>
                <li><strong>生命：</strong>你有3条生命，用完则游戏结束</li>
            </ul>
            <p>提示：马里奥在移动中可以跳得更高更远！</p>
        </div>
        
        <div class="footer">
            <p>基于经典马里奥游戏制作的简化版本 | 使用HTML5 Canvas开发</p>
        </div>
    </div>

    <script>
        // 游戏主对象
        const game = {
            canvas: null,
            ctx: null,
            width: 800,
            height: 400,
            scale: 1,
            currentLevel: 1,
            coins: 0,
            lives: 3,
            time: 300,
            score: 0,
            gameRunning: false,
            gameOver: false,
            levelComplete: false,
            keys: {},
            lastTime: 0,
            gravity: 0.5,
            friction: 0.8,
            player: null,
            platforms: [],
            enemies: [],
            coinsList: [],
            flag: null,
            levelCoins: 0,
            
            init() {
                this.canvas = document.getElementById('gameCanvas');
                this.ctx = this.canvas.getContext('2d');
                
                // 设置Canvas尺寸
                this.resizeCanvas();
                window.addEventListener('resize', () => this.resizeCanvas());
                
                // 初始化玩家
                this.player = {
                    x: 50,
                    y: this.height - 100,
                    width: 30,
                    height: 40,
                    velX: 0,
                    velY: 0,
                    speed: 5,
                    jumpPower: 12,
                    jumping: false,
                    direction: 'right',
                    color: '#E52521' // 马里奥红色
                };
                
                // 设置关卡
                this.setupLevel(this.currentLevel);
                
                // 设置游戏循环
                this.gameLoop();
                
                // 初始化事件监听器
                this.setupEventListeners();
                
                // 更新时间显示
                this.updateDisplay();
                
                // 开始时间倒计时
                setInterval(() => {
                    if (this.gameRunning && !this.levelComplete && !this.gameOver) {
                        this.time--;
                        this.updateDisplay();
                        
                        if (this.time <= 0) {
                            this.gameOver = true;
                            this.gameRunning = false;
                            document.getElementById('gameOverMessage').textContent = "时间用完了！";
                            document.getElementById('gameOverScreen').classList.remove('hidden');
                        }
                    }
                }, 1000);
            },
            
            resizeCanvas() {
                const container = document.querySelector('.game-container');
                const containerWidth = container.clientWidth;
                
                this.scale = containerWidth / this.width;
                this.canvas.width = this.width;
                this.canvas.height = this.height;
                
                // 设置CSS尺寸以保持高宽比
                this.canvas.style.width = `${containerWidth}px`;
                this.canvas.style.height = `${this.height * this.scale}px`;
            },
            
            setupEventListeners() {
                // 键盘控制
                window.addEventListener('keydown', (e) => {
                    this.keys[e.key] = true;
                    
                    // 空格键跳跃
                    if (e.key === ' ' && this.gameRunning && !this.player.jumping) {
                        this.player.velY = -this.player.jumpPower;
                        this.player.jumping = true;
                    }
                    
                    // 防止空格键滚动页面
                    if (e.key === ' ') {
                        e.preventDefault();
                    }
                });
                
                window.addEventListener('keyup', (e) => {
                    this.keys[e.key] = false;
                });
                
                // 触摸/按钮控制
                document.getElementById('startBtn').addEventListener('click', () => this.startGame());
                document.getElementById('restartBtn').addEventListener('click', () => this.restartGame());
                document.getElementById('nextLevelBtn').addEventListener('click', () => this.nextLevel());
                
                // 移动控制按钮
                const leftBtn = document.getElementById('leftBtn');
                const rightBtn = document.getElementById('rightBtn');
                const upBtn = document.getElementById('upBtn');
                const jumpBtn = document.getElementById('jumpBtn');
                
                // 左按钮
                leftBtn.addEventListener('touchstart', (e) => {
                    e.preventDefault();
                    this.keys['ArrowLeft'] = true;
                });
                leftBtn.addEventListener('touchend', (e) => {
                    e.preventDefault();
                    this.keys['ArrowLeft'] = false;
                });
                leftBtn.addEventListener('mousedown', () => {
                    this.keys['ArrowLeft'] = true;
                });
                leftBtn.addEventListener('mouseup', () => {
                    this.keys['ArrowLeft'] = false;
                });
                
                // 右按钮
                rightBtn.addEventListener('touchstart', (e) => {
                    e.preventDefault();
                    this.keys['ArrowRight'] = true;
                });
                rightBtn.addEventListener('touchend', (e) => {
                    e.preventDefault();
                    this.keys['ArrowRight'] = false;
                });
                rightBtn.addEventListener('mousedown', () => {
                    this.keys['ArrowRight'] = true;
                });
                rightBtn.addEventListener('mouseup', () => {
                    this.keys['ArrowRight'] = false;
                });
                
                // 上按钮（跳跃）
                upBtn.addEventListener('touchstart', (e) => {
                    e.preventDefault();
                    if (this.gameRunning && !this.player.jumping) {
                        this.player.velY = -this.player.jumpPower;
                        this.player.jumping = true;
                    }
                });
                upBtn.addEventListener('mousedown', () => {
                    if (this.gameRunning && !this.player.jumping) {
                        this.player.velY = -this.player.jumpPower;
                        this.player.jumping = true;
                    }
                });
                
                // 跳跃按钮
                jumpBtn.addEventListener('touchstart', (e) => {
                    e.preventDefault();
                    if (this.gameRunning && !this.player.jumping) {
                        this.player.velY = -this.player.jumpPower;
                        this.player.jumping = true;
                    }
                });
                jumpBtn.addEventListener('mousedown', () => {
                    if (this.gameRunning && !this.player.jumping) {
                        this.player.velY = -this.player.jumpPower;
                        this.player.jumping = true;
                    }
                });
                
                // 防止按钮的默认行为
                [leftBtn, rightBtn, upBtn, jumpBtn].forEach(btn => {
                    btn.addEventListener('contextmenu', (e) => e.preventDefault());
                });
            },
            
            startGame() {
                this.gameRunning = true;
                this.gameOver = false;
                this.levelComplete = false;
                document.getElementById('startScreen').classList.add('hidden');
                document.getElementById('gameOverScreen').classList.add('hidden');
                document.getElementById('levelCompleteScreen').classList.add('hidden');
            },
            
            restartGame() {
                this.currentLevel = 1;
                this.coins = 0;
                this.lives = 3;
                this.time = 300;
                this.score = 0;
                this.setupLevel(this.currentLevel);
                this.startGame();
                this.updateDisplay();
            },
            
            nextLevel() {
                this.currentLevel++;
                this.time = 300;
                this.setupLevel(this.currentLevel);
                this.startGame();
                this.updateDisplay();
            },
            
            updateDisplay() {
                document.getElementById('level').textContent = this.currentLevel;
                document.getElementById('coins').textContent = this.coins;
                document.getElementById('lives').textContent = this.lives;
                document.getElementById('time').textContent = this.time;
            },
            
            setupLevel(level) {
                // 重置游戏对象
                this.platforms = [];
                this.enemies = [];
                this.coinsList = [];
                
                // 根据关卡设置不同的难度
                const enemyCount = Math.min(level + 1, 5);
                this.levelCoins = Math.min(level * 5, 20);
                
                // 地面
                this.platforms.push({
                    x: 0,
                    y: this.height - 40,
                    width: this.width,
                    height: 40,
                    color: '#8B4513'
                });
                
                // 添加一些平台
                // 第一个平台
                this.platforms.push({
                    x: 200,
                    y: this.height - 120,
                    width: 100,
                    height: 20,
                    color: '#1E7F1E'
                });
                
                // 第二个平台
                this.platforms.push({
                    x: 400,
                    y: this.height - 160,
                    width: 100,
                    height: 20,
                    color: '#1E7F1E'
                });
                
                // 第三个平台
                this.platforms.push({
                    x: 600,
                    y: this.height - 200,
                    width: 100,
                    height: 20,
                    color: '#1E7F1E'
                });
                
                // 添加管道（特殊平台）
                this.platforms.push({
                    x: 300,
                    y: this.height - 100,
                    width: 60,
                    height: 60,
                    color: '#32CD32'
                });
                
                // 添加敌人
                for (let i = 0; i < enemyCount; i++) {
                    this.enemies.push({
                        x: 300 + i * 80,
                        y: this.height - 80,
                        width: 30,
                        height: 30,
                        velX: (i % 2 === 0) ? 2 : -2,
                        color: '#800080', // 紫色敌人
                        type: 'goomba'
                    });
                }
                
                // 添加金币
                for (let i = 0; i < this.levelCoins; i++) {
                    this.coinsList.push({
                        x: 150 + i * 30,
                        y: this.height - 150,
                        width: 15,
                        height: 15,
                        collected: false
                    });
                    
                    // 随机放置一些金币在平台上
                    if (i % 3 === 0 && i < 15) {
                        this.coinsList.push({
                            x: 210 + (i * 40),
                            y: this.height - 140,
                            width: 15,
                            height: 15,
                            collected: false
                        });
                    }
                }
                
                // 添加终点旗杆
                this.flag = {
                    x: this.width - 60,
                    y: this.height - 180,
                    width: 10,
                    height: 100,
                    reached: false
                };
                
                // 重置玩家位置
                this.player.x = 50;
                this.player.y = this.height - 100;
                this.player.velX = 0;
                this.player.velY = 0;
                this.player.jumping = false;
            },
            
            gameLoop(timestamp) {
                // 计算时间增量
                const deltaTime = timestamp - this.lastTime || 0;
                this.lastTime = timestamp;
                
                // 清除画布
                this.ctx.clearRect(0, 0, this.width, this.height);
                
                // 绘制背景
                this.drawBackground();
                
                // 如果游戏正在运行，更新游戏状态
                if (this.gameRunning && !this.levelComplete && !this.gameOver) {
                    this.updatePlayer();
                    this.updateEnemies();
                    this.checkCollisions();
                }
                
                // 绘制游戏对象
                this.drawPlatforms();
                this.drawCoins();
                this.drawEnemies();
                this.drawFlag();
                this.drawPlayer();
                
                // 继续游戏循环
                requestAnimationFrame((ts) => this.gameLoop(ts));
            },
            
            drawBackground() {
                // 天空
                this.ctx.fillStyle = '#87CEEB';
                this.ctx.fillRect(0, 0, this.width, this.height);
                
                // 云朵
                this.ctx.fillStyle = 'white';
                this.ctx.beginPath();
                this.ctx.arc(100, 60, 20, 0, Math.PI * 2);
                this.ctx.arc(130, 60, 25, 0, Math.PI * 2);
                this.ctx.arc(160, 60, 20, 0, Math.PI * 2);
                this.ctx.fill();
                
                this.ctx.beginPath();
                this.ctx.arc(500, 90, 20, 0, Math.PI * 2);
                this.ctx.arc(530, 90, 25, 0, Math.PI * 2);
                this.ctx.arc(560, 90, 20, 0, Math.PI * 2);
                this.ctx.fill();
                
                // 远处的山
                this.ctx.fillStyle = '#2E8B57';
                this.ctx.beginPath();
                this.ctx.moveTo(0, this.height - 40);
                this.ctx.lineTo(150, this.height - 140);
                this.ctx.lineTo(300, this.height - 40);
                this.ctx.fill();
                
                this.ctx.beginPath();
                this.ctx.moveTo(250, this.height - 40);
                this.ctx.lineTo(400, this.height - 160);
                this.ctx.lineTo(550, this.height - 40);
                this.ctx.fill();
            },
            
            drawPlatforms() {
                this.platforms.forEach(platform => {
                    this.ctx.fillStyle = platform.color;
                    this.ctx.fillRect(platform.x, platform.y, platform.width, platform.height);
                    
                    // 添加平台纹理
                    this.ctx.fillStyle = platform.color === '#1E7F1E' ? '#145214' : '#5D2906';
                    for (let i = 0; i < platform.width; i += 20) {
                        this.ctx.fillRect(platform.x + i, platform.y, 10, 5);
                    }
                });
            },
            
            drawCoins() {
                this.coinsList.forEach(coin => {
                    if (!coin.collected) {
                        // 绘制金币
                        this.ctx.fillStyle = '#FFD700';
                        this.ctx.beginPath();
                        this.ctx.ellipse(coin.x + coin.width/2, coin.y + coin.height/2, coin.width/2, coin.height/2, 0, 0, Math.PI * 2);
                        this.ctx.fill();
                        
                        this.ctx.fillStyle = '#FFA500';
                        this.ctx.beginPath();
                        this.ctx.ellipse(coin.x + coin.width/2, coin.y + coin.height/2, coin.width/3, coin.height/3, 0, 0, Math.PI * 2);
                        this.ctx.fill();
                    }
                });
            },
            
            drawEnemies() {
                this.enemies.forEach(enemy => {
                    this.ctx.fillStyle = enemy.color;
                    
                    if (enemy.type === 'goomba') {
                        // 绘制蘑菇敌人
                        this.ctx.beginPath();
                        this.ctx.ellipse(
                            enemy.x + enemy.width/2, 
                            enemy.y + enemy.height/2, 
                            enemy.width/2, 
                            enemy.height/2, 
                            0, 0, Math.PI * 2
                        );
                        this.ctx.fill();
                        
                        // 眼睛
                        this.ctx.fillStyle = 'white';
                        this.ctx.beginPath();
                        this.ctx.ellipse(
                            enemy.x + enemy.width/3, 
                            enemy.y + enemy.height/3, 
                            3, 3, 0, 0, Math.PI * 2
                        );
                        this.ctx.fill();
                        
                        this.ctx.beginPath();
                        this.ctx.ellipse(
                            enemy.x + 2*enemy.width/3, 
                            enemy.y + enemy.height/3, 
                            3, 3, 0, 0, Math.PI * 2
                        );
                        this.ctx.fill();
                    }
                });
            },
            
            drawFlag() {
                // 旗杆
                this.ctx.fillStyle = '#808080';
                this.ctx.fillRect(this.flag.x, this.flag.y, this.flag.width, this.flag.height);
                
                // 旗子
                this.ctx.fillStyle = this.flag.reached ? '#32CD32' : '#E52521';
                this.ctx.beginPath();
                this.ctx.moveTo(this.flag.x + this.flag.width, this.flag.y + 10);
                this.ctx.lineTo(this.flag.x + this.flag.width + 30, this.flag.y + 20);
                this.ctx.lineTo(this.flag.x + this.flag.width, this.flag.y + 30);
                this.ctx.closePath();
                this.ctx.fill();
            },
            
            drawPlayer() {
                // 绘制马里奥
                this.ctx.fillStyle = this.player.color;
                this.ctx.fillRect(this.player.x, this.player.y, this.player.width, this.player.height);
                
                // 马里奥的脸
                this.ctx.fillStyle = '#FFE4B5'; // 肤色
                this.ctx.fillRect(this.player.x + 5, this.player.y + 5, this.player.width - 10, 15);
                
                // 帽子
                this.ctx.fillStyle = '#E52521';
                this.ctx.fillRect(this.player.x, this.player.y, this.player.width, 8);
                this.ctx.fillRect(this.player.x + 5, this.player.y - 5, 20, 10);
                
                // 眼睛
                this.ctx.fillStyle = 'white';
                this.ctx.fillRect(this.player.x + 10, this.player.y + 8, 4, 4);
                this.ctx.fillRect(this.player.x + 20, this.player.y + 8, 4, 4);
                
                // 嘴巴
                this.ctx.fillStyle = '#8B0000';
                this.ctx.fillRect(this.player.x + 10, this.player.y + 15, 15, 2);
                
                // 裤子
                this.ctx.fillStyle = '#1E7F1E';
                this.ctx.fillRect(this.player.x + 5, this.player.y + 30, this.player.width - 10, 10);
            },
            
            updatePlayer() {
                // 水平移动
                if (this.keys['ArrowLeft'] || this.keys['a'] || this.keys['A']) {
                    this.player.velX = -this.player.speed;
                    this.player.direction = 'left';
                } else if (this.keys['ArrowRight'] || this.keys['d'] || this.keys['D']) {
                    this.player.velX = this.player.speed;
                    this.player.direction = 'right';
                } else {
                    this.player.velX *= this.friction;
                }
                
                // 应用重力
                this.player.velY += this.gravity;
                
                // 更新位置
                this.player.x += this.player.velX;
                this.player.y += this.player.velY;
                
                // 边界检查
                if (this.player.x < 0) this.player.x = 0;
                if (this.player.x + this.player.width > this.width) this.player.x = this.width - this.player.width;
                
                // 检查是否掉出屏幕
                if (this.player.y > this.height) {
                    this.loseLife();
                }
                
                // 平台碰撞检测
                this.player.jumping = true;
                
                this.platforms.forEach(platform => {
                    if (this.collision(this.player, platform)) {
                        // 从上方碰撞平台
                        if (this.player.velY > 0 && this.player.y + this.player.height - this.player.velY <= platform.y) {
                            this.player.y = platform.y - this.player.height;
                            this.player.velY = 0;
                            this.player.jumping = false;
                        }
                        // 从下方碰撞平台
                        else if (this.player.velY < 0 && this.player.y - this.player.velY >= platform.y + platform.height) {
                            this.player.y = platform.y + platform.height;
                            this.player.velY = 0;
                        }
                        // 从侧面碰撞平台
                        else if (this.player.velX > 0 && this.player.x + this.player.width - this.player.velX <= platform.x) {
                            this.player.x = platform.x - this.player.width;
                            this.player.velX = 0;
                        }
                        else if (this.player.velX < 0 && this.player.x - this.player.velX >= platform.x + platform.width) {
                            this.player.x = platform.x + platform.width;
                            this.player.velX = 0;
                        }
                    }
                });
            },
            
            updateEnemies() {
                this.enemies.forEach(enemy => {
                    // 移动敌人
                    enemy.x += enemy.velX;
                    
                    // 敌人边界检查
                    if (enemy.x < 0 || enemy.x + enemy.width > this.width) {
                        enemy.velX *= -1;
                    }
                    
                    // 敌人平台碰撞
                    let onPlatform = false;
                    this.platforms.forEach(platform => {
                        if (this.collision(enemy, platform)) {
                            // 从上方碰撞平台
                            if (enemy.y + enemy.height <= platform.y + 5) {
                                enemy.y = platform.y - enemy.height;
                                onPlatform = true;
                            }
                            // 从侧面碰撞平台
                            else if (enemy.velX > 0 && enemy.x + enemy.width <= platform.x + 5) {
                                enemy.velX *= -1;
                            }
                            else if (enemy.velX < 0 && enemy.x >= platform.x + platform.width - 5) {
                                enemy.velX *= -1;
                            }
                        }
                    });
                    
                    // 如果敌人不在平台上，让它下落
                    if (!onPlatform && enemy.y + enemy.height < this.height - 40) {
                        enemy.y += 5;
                    }
                });
            },
            
            checkCollisions() {
                // 检查与敌人的碰撞
                this.enemies.forEach((enemy, index) => {
                    if (this.collision(this.player, enemy)) {
                        // 如果玩家从上方跳到敌人头上
                        if (this.player.velY > 0 && this.player.y + this.player.height - this.player.velY <= enemy.y + 10) {
                            // 消灭敌人
                            this.enemies.splice(index, 1);
                            this.player.velY = -this.player.jumpPower * 0.8;
                            this.score += 100;
                        } else {
                            // 玩家被敌人击中
                            this.loseLife();
                        }
                    }
                });
                
                // 检查与金币的碰撞
                this.coinsList.forEach((coin, index) => {
                    if (!coin.collected && this.collision(this.player, coin)) {
                        coin.collected = true;
                        this.coins++;
                        this.score += 50;
                        this.updateDisplay();
                    }
                });
                
                // 检查是否到达旗杆
                if (!this.flag.reached && this.collision(this.player, this.flag)) {
                    this.flag.reached = true;
                    this.completeLevel();
                }
            },
            
            collision(rect1, rect2) {
                return rect1.x < rect2.x + rect2.width &&
                       rect1.x + rect1.width > rect2.x &&
                       rect1.y < rect2.y + rect2.height &&
                       rect1.y + rect1.height > rect2.y;
            },
            
            loseLife() {
                this.lives--;
                this.updateDisplay();
                
                if (this.lives <= 0) {
                    this.gameOver = true;
                    this.gameRunning = false;
                    document.getElementById('gameOverMessage').textContent = "游戏结束！你失去了所有生命。";
                    document.getElementById('gameOverScreen').classList.remove('hidden');
                } else {
                    // 重置玩家位置
                    this.player.x = 50;
                    this.player.y = this.height - 100;
                    this.player.velX = 0;
                    this.player.velY = 0;
                    this.player.jumping = false;
                }
            },
            
            completeLevel() {
                this.levelComplete = true;
                this.gameRunning = false;
                
                // 计算奖励分数
                const timeBonus = Math.max(0, this.time) * 10;
                const coinBonus = this.coins * 100;
                const levelBonus = this.currentLevel * 500;
                this.score += timeBonus + coinBonus + levelBonus;
                
                document.getElementById('finalCoins').textContent = this.coins;
                document.getElementById('levelCompleteMessage').textContent = 
                    `你收集了 ${this.coins} 枚金币，获得 ${timeBonus} 时间奖励分！`;
                
                // 如果是最后一关，显示不同的消息
                if (this.currentLevel >= 3) {
                    document.getElementById('levelCompleteMessage').textContent += 
                        ` 恭喜你完成了所有关卡！最终得分: ${this.score}`;
                    document.getElementById('nextLevelBtn').textContent = "重新开始";
                }
                
                document.getElementById('levelCompleteScreen').classList.remove('hidden');
            }
        };
        
        // 初始化游戏
        window.addEventListener('load', () => {
            game.init();
        });
    </script>
</body>
</html>
