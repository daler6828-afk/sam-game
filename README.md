<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>NECRO-CRAFT MOBILE</title>
    <style>
        body { margin: 0; background: #000; color: #fff; font-family: sans-serif; overflow: hidden; touch-action: none; }
        
        /* Верхняя панель для телефона */
        #ui-top {
            position: absolute; top: 0; left: 0; width: 100%; height: 40px;
            background: rgba(0,0,0,0.7); display: flex; justify-content: space-around;
            align-items: center; font-size: 12px; z-index: 10; border-bottom: 2px solid #555;
        }

        /* Игровое поле */
        #game-area { width: 100vw; height: 100vh; background: #111; }
        canvas { display: block; }

        /* Мобильный инвентарь снизу */
        #hotbar-container {
            position: absolute; bottom: 10px; left: 50%; transform: translateX(-50%);
            width: 95%; max-width: 500px; z-index: 20;
        }

        #hotbar {
            display: grid; grid-template-columns: repeat(5, 1fr); gap: 5px;
            background: #c6c6c6; padding: 5px; border: 3px solid #555;
            box-shadow: inset -2px -2px 0 #333, inset 2px 2px 0 #fff;
        }

        .slot {
            aspect-ratio: 1/1; background: #8b8b8b; border: 2px solid #333;
            display: flex; align-items: center; justify-content: center;
            font-size: 24px; position: relative;
        }
        .slot.active { border-color: #ffff00; background: #aa4444; box-shadow: 0 0 10px #ff0; }
        .price { position: absolute; bottom: 1px; right: 2px; font-size: 8px; color: #fff; text-shadow: 1px 1px 1px #000; }

        /* Кнопки магии по бокам */
        .magic-btn {
            position: absolute; right: 10px; bottom: 120px;
            width: 50px; height: 50px; background: rgba(0,100,255,0.6);
            border: 2px solid #0af; border-radius: 50%;
            display: flex; align-items: center; justify-content: center; font-size: 20px;
        }
        #btn-meteor { bottom: 180px; background: rgba(255,100,0,0.6); border-color: #f60; }
    </style>
</head>
<body>

<div id="ui-top">
    <div>💎 <span id="souls">1500</span></div>
    <div>❤️ <span id="hp">100</span>%</div>
    <div>🌊 ВОЛНА: <span id="wave">1</span></div>
    <div id="timer">15s</div>
</div>

<div id="game-area">
    <canvas id="game"></canvas>
</div>

<div id="hotbar-container">
    <div id="hotbar">
        <div class="slot active" onclick="setT('wall', 15, this)">🧱<div class="price">15</div></div>
        <div class="slot" onclick="setT('obsid', 200, this)">⬛<div class="price">200</div></div>
        <div class="slot" onclick="setT('turret', 120, this)">🗼<div class="price">120</div></div>
        <div class="slot" onclick="setT('tnt', 100, this)">🧨<div class="price">100</div></div>
        <div class="slot" onclick="setT('torch', 5, this)">🔥<div class="price">5</div></div>
    </div>
</div>

<div class="magic-btn" id="btn-meteor" onclick="cast('meteor')">☄️</div>
<div class="magic-btn" onclick="cast('freeze')">❄️</div>

<script>
    const canvas = document.getElementById('game');
    const ctx = canvas.getContext('2d');
    
    let souls = 1000, hp = 100, wave = 1, timer = 15, tool = 'wall', cost = 15;
    let tileSize = 40; 
    let cols, rows, grid = [], enemies = [], projectiles = [];
    let freezeTime = 0;
    let heart = {x: 0, y: 0};

    function init() {
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
        // На телефоне сетка чуть крупнее, чтобы попадать пальцем
        tileSize = Math.min(canvas.width, canvas.height) / 10;
        cols = Math.floor(canvas.width / tileSize);
        rows = Math.floor(canvas.height / tileSize);
        
        grid = [];
        for(let y=0; y<rows; y++) {
            grid[y] = [];
            for(let x=0; x<cols; x++) grid[y][x] = {type: 'floor', hp: 100, last: 0};
        }
        heart = {x: cols-2, y: Math.floor(rows/2)};
        grid[heart.y][heart.x].type = 'heart';
    }

    function setT(t, c, el) {
        tool = t; cost = c;
        document.querySelectorAll('.slot').forEach(s => s.classList.remove('active'));
        el.classList.add('active');
    }

    // Обработка касаний (Touch)
    function handleInput(e) {
        e.preventDefault();
        const touch = e.touches ? e.touches[0] : e;
        const x = Math.floor(touch.clientX / tileSize);
        const y = Math.floor(touch.clientY / tileSize);
        
        if(grid[y] && grid[y][x]) {
            if(grid[y][x].type === 'floor' && souls >= cost) {
                souls -= cost;
                grid[y][x].type = tool;
                grid[y][x].hp = (tool === 'obsid' ? 2500 : 800);
            } else if(grid[y][x].type !== 'heart' && grid[y][x].type !== 'floor') {
                grid[y][x].type = 'floor';
            }
        }
    }

    canvas.addEventListener('touchstart', handleInput);
    canvas.addEventListener('mousedown', handleInput);

    function cast(type) {
        if(type === 'freeze' && souls >= 120) { souls -= 120; freezeTime = 180; }
        if(type === 'meteor' && souls >= 200) { 
            souls -= 200; 
            enemies.forEach(en => { if(en.x < cols/2) en.hp -= 2000; });
        }
    }

    function update() {
        if(freezeTime > 0) freezeTime--;
        enemies.forEach((en, i) => {
            if(freezeTime > 0) return;
            let dx = heart.x - en.x, dy = heart.y - en.y;
            let dist = Math.hypot(dx, dy);
            let gx = Math.floor(en.x + (dx/dist)*0.4), gy = Math.floor(en.y + (dy/dist)*0.4);
            
            let cell = (grid[gy] && grid[gy][gx]) ? grid[gy][gx] : null;
            if(cell && cell.type !== 'floor' && cell.type !== 'heart') {
                cell.hp -= 5;
                if(cell.hp <= 0) {
                    if(cell.type === 'tnt') {
                        enemies.forEach(e => { if(Math.hypot(e.x-gx, e.y-gy) < 3) e.hp -= 1000; });
                    }
                    cell.type = 'floor';
                }
            } else {
                en.x += (dx/dist) * en.s; en.y += (dy/dist) * en.s;
            }

            if(dist < 0.5) { hp -= 10; enemies.splice(i, 1); }
            if(en.hp <= 0) { enemies.splice(i, 1); souls += 30; }
        });

        grid.forEach((row, y) => row.forEach((c, x) => {
            if(c.type === 'turret' && Date.now() - c.last > 1000) {
                let target = enemies.find(en => Math.hypot(en.x-x, en.y-y) < 7);
                if(target) { 
                    target.hp -= 70; c.last = Date.now(); 
                    projectiles.push({sx: x, sy: y, tx: target.x, ty: target.y, life: 5});
                }
            }
        }));
        
        document.getElementById('souls').innerText = Math.floor(souls);
        document.getElementById('hp').innerText = Math.max(0, hp);
        if(hp <= 0) location.reload();
    }

    function draw() {
        ctx.fillStyle = freezeTime > 0 ? '#001a33' : '#111';
        ctx.fillRect(0,0,canvas.width,canvas.height);
        
        for(let y=0; y<rows; y++) for(let x=0; x<cols; x++) {
            let c = grid[y][x], px = x*tileSize, py = y*tileSize;
            if(c.type === 'heart') { ctx.fillStyle = '#f00'; ctx.fillRect(px+2, py+2, tileSize-4, tileSize-4); }
            if(c.type === 'wall') { ctx.fillStyle = '#8b8b8b'; ctx.fillRect(px+4, py+4, tileSize-8, tileSize-8); }
            if(c.type === 'obsid') { ctx.fillStyle = '#203'; ctx.fillRect(px+2, py+2, tileSize-4, tileSize-4); }
            if(c.type === 'turret') { ctx.fillStyle = '#70f'; ctx.beginPath(); ctx.arc(px+tileSize/2, py+tileSize/2, tileSize/3, 0, 7); ctx.fill(); }
            if(c.type === 'tnt') { ctx.fillStyle = '#f00'; ctx.fillRect(px+4, py+4, tileSize-8, tileSize-8); }
            if(c.type === 'torch') { ctx.fillStyle = '#f60'; ctx.fillRect(px+tileSize/2-2, py+10, 4, 20); }
        }

        enemies.forEach(en => {
            ctx.fillStyle = en.color;
            ctx.fillRect(en.x*tileSize, en.y*tileSize, tileSize/2, tileSize/2);
        });

        projectiles.forEach((p, i) => {
            ctx.strokeStyle = '#f0f'; ctx.lineWidth = 2;
            ctx.beginPath(); ctx.moveTo(p.sx*tileSize+tileSize/2, p.sy*tileSize+tileSize/2);
            ctx.lineTo(p.tx*tileSize+tileSize/2, p.ty*tileSize+tileSize/2); ctx.stroke();
            p.life--; if(p.life <= 0) projectiles.splice(i, 1);
        });

        update(); requestAnimationFrame(draw);
    }

    setInterval(() => {
        timer--;
        if(timer <= 0) {
            timer = 15; wave++; document.getElementById('wave').innerText = wave;
            for(let i=0; i < 4 + wave; i++) {
                setTimeout(() => {
                    enemies.push({ x: 0, y: Math.random()*rows, hp: 150 + wave*20, s: 0.04, color: '#fff' });
                }, i * 400);
            }
        }
        document.getElementById('timer').innerText = timer + 's';
    }, 1000);

    window.addEventListener('resize', init);
    init(); draw();
</script>
</body>
</html>
