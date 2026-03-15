<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>GAME SAM FIXED</title>
    <style>
        :root { --font: 'Segoe UI', sans-serif; }
        .theme-space { --bg: #0f172a; --card: #1e293b; --accent: #38bdf8; --text: #f8fafc; --tile: #334155; }
        .theme-forest { --bg: #064e3b; --card: #065f46; --accent: #4ade80; --text: #ecfdf5; --tile: #064e3b; }
        .theme-volcano { --bg: #1a0b0b; --card: #2d1616; --accent: #f87171; --text: #fee2e2; --tile: #451a1a; }
        .theme-cyberpunk { --bg: #160025; --card: #2d004d; --accent: #ff00ff; --text: #fff; --tile: #3d0066; }
        .theme-matrix { --bg: #000; --card: #0d0d0d; --accent: #00ff41; --text: #00ff41; --tile: #1a1a1a; --font: 'Courier New', monospace; }

        * { box-sizing: border-box; -webkit-user-select: none; touch-action: none; }
        body { 
            font-family: var(--font); background: var(--bg); color: var(--text); 
            margin: 0; padding: 0; height: 100dvh; width: 100vw; 
            overflow: hidden; display: flex; flex-direction: column; transition: 0.3s;
        }

        .panel { 
            background: var(--card); padding: 20px; border-radius: 25px; 
            width: 92%; max-width: 450px; margin: auto; border: 2px solid var(--accent);
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        }
        .setup-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 15px; }
        label { font-size: 10px; font-weight: 800; color: var(--accent); text-transform: uppercase; }
        input, select { width: 100%; padding: 10px; border-radius: 10px; border: 1px solid var(--accent); background: rgba(0,0,0,0.3); color: #fff; }

        #game-ui { display: none; flex: 1; width: 100%; height: 100%; overflow: hidden; }

        @media (orientation: landscape) {
            #game-ui { flex-direction: row; }
            .scoreboard { width: 130px; height: 100%; display: flex; flex-direction: column; gap: 8px; padding: 12px; background: rgba(0,0,0,0.3); flex-shrink: 0; border-right: 1px solid var(--accent); }
            .grid-container { flex: 1; height: 100%; position: relative; }
        }

        @media (orientation: portrait) {
            #game-ui { flex-direction: column; }
            .scoreboard { display: flex; gap: 8px; padding: 10px; background: rgba(0,0,0,0.2); flex-shrink: 0; overflow-x: auto; }
            .grid-container { flex: 1; position: relative; }
        }

        .team-box { background: var(--card); border-radius: 12px; text-align: center; border: 2px solid transparent; padding: 8px; }
        .team-box.active { border-color: var(--accent); box-shadow: 0 0 15px var(--accent); }
        .score { font-size: 20px; font-weight: 900; color: var(--accent); }

        .grid { 
            position: absolute; top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            display: grid; gap: 8px; justify-content: center; align-content: center;
        }

        .tile { 
            background: var(--tile); border-radius: 12px; 
            display: flex; align-items: center; justify-content: center; 
            font-weight: 900; border-bottom: 4px solid rgba(0,0,0,0.4);
            cursor: pointer; transition: 0.1s;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
        }
        .tile:active { transform: scale(0.9); border-bottom-width: 0; }
        .tile.used { visibility: hidden; pointer-events: none; }

        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.96); z-index: 100; align-items: center; justify-content: center; padding: 20px; }
        .modal-content { background: var(--card); width: 100%; max-width: 450px; border-radius: 30px; padding: 25px; text-align: center; border: 3px solid var(--accent); }
        .btn { padding: 15px; border-radius: 15px; border: none; font-weight: 900; width: 100%; cursor: pointer; text-transform: uppercase; }
        .btn-check { background: var(--accent); color: #000; margin-top: 10px; }
    </style>
</head>
<body class="theme-space" id="main-body">

<div class="panel" id="setup">
    <h2 style="text-align:center; color:var(--accent); margin-top:0;">GAME SAM FIXED</h2>
    <div class="setup-grid" style="grid-template-columns: 1fr;"><label>Тема</label>
        <select onchange="document.getElementById('main-body').className = this.value">
            <option value="theme-space">🪐 Космос</option><option value="theme-forest">🌲 Лес</option><option value="theme-volcano">🌋 Вулкан</option><option value="theme-cyberpunk">🧬 Киберпанк</option><option value="theme-matrix">💾 Матрица</option>
        </select>
    </div>
    <div class="setup-grid">
        <div style="grid-column: span 2;"><label>Команды</label><input type="text" id="v-teams" value="Сэм, Друзья"></div>
        <div><label>Карт всего</label><input type="number" id="v-total" value="20"></div>
        <div><label>Верно (+)</label><input type="number" id="v-win" value="15"></div>
        <div><label>Бомба (-)</label><input type="number" id="v-bomb" value="20"></div>
    </div>
    <label>Бонусы</label>
    <div class="setup-grid" style="grid-template-columns: repeat(4, 1fr);">
        <div><label>💣</label><input type="number" id="c-b" value="2"></div>
        <div><label>💎</label><input type="number" id="c-g" value="1"></div>
        <div><label>🚀</label><input type="number" id="c-r" value="1"></div>
        <div><label>🎲</label><input type="number" id="c-l" value="1"></div>
    </div>
    <button class="btn btn-check" onclick="startWithFS()">ПОЕХАЛИ!</button>
</div>

<div id="game-ui">
    <div class="scoreboard" id="score-board"></div>
    <div class="grid-container" id="grid-parent"><div class="grid" id="game-grid"></div></div>
</div>

<div id="modal" class="modal">
    <div class="modal-content">
        <div id="m-icon" style="font-size:60px">❓</div>
        <p id="m-text" style="font-size:24px; font-weight: 800; margin:20px 0;"></p>
        <button id="m-rev-btn" class="btn btn-check" onclick="showAns()">ОТВЕТ</button>
        <div id="m-ans-box" style="display:none; padding:15px; background:rgba(0,0,0,0.2); border-radius:15px; border:2px solid var(--accent); margin-bottom:15px;"><b id="m-ans-text" style="color:var(--accent)"></b></div>
        <div id="m-ctrl" style="display:none; gap:10px;"><button class="btn" style="background:#22c55e; color:#fff" onclick="endTurn(true)">ВЕРНО</button><button class="btn" style="background:#ef4444; color:#fff" onclick="endTurn(false)">НЕТ</button></div>
        <button id="m-ok-btn" class="btn btn-check" style="display:none" onclick="nextPlayer()">ПРОДОЛЖИТЬ</button>
    </div>
</div>

<script>
const chains = ["О борьбе между Христом и сатаной в теч 7000 лет", "Пророчество Еноха", "Пророчество о потопе", "Пророчество Ноя о его потомках", "430 лет", "Пророчество Иакова", "Пророчество Иова", "40 лет странствования по пустыне", "Пророчество Моисея о Христе как пророке", "70 лет Вавилонского плена", "Великий пророческий истукан", "Четыре больших зверя", "Овен и козёл", "2300 вечеров и утр (лет)", "70 седьмин", "1260 дней (лет)", "1290 дней (лет)", "1335 дней (лет)", "Великое пророчество Иисуса Христа", "Пророч. ап. Павла о вел. отступл. в христ-ве", "Семь церквей", "Семь печатей", "Удержание ветров и окончание запечатления", "Семь труб", "Время Филадельф. Адвент. в симв. Ангела обл. в обл.", "Особое время, не измеряемое тростью ВНЗ", "1260 лет Библия во вретище", "Церковь Божья всех веков", "Время папского Рима", "Двурогий зверь - лжепротестантизм", "Время формирован. и запечатл. 144 000 искупл.", "Время трёхангельской вести", "Две жатвы", "Песнь Моисея и Агнца перед язвами", "Закрытие благодати", "Излитие семи посл. чаш (язв) гнева Б. на землю", "Два зверя и их участие в последней вел. борьбе", "Весть Иного Ангела. Посл. предупрежд. миру", "Наказание Вавилона", "Радость неба о правосудии над Вавилоном", "4 посл. вест. имени Хр. и 4 стадии раб. Иного Анг.", "Великая вечеря птиц", "1000-летнее царство и суд", "Город Новый Иерусалим - Жена невеста Агнца"];
let state = { teams:[], scores:[], cur:0, deck:[], wP:15, bP:20, extra:false };

function startWithFS() {
    let e = document.documentElement;
    if (e.requestFullscreen) e.requestFullscreen();
    else if (e.webkitRequestFullscreen) e.webkitRequestFullscreen();
    startGame();
}

function startGame() {
    state.teams = document.getElementById('v-teams').value.split(',').map(s => s.trim()).filter(s => s);
    state.scores = state.teams.map(() => 0);
    state.wP = parseInt(document.getElementById('v-win').value);
    state.bP = parseInt(document.getElementById('v-bomb').value);
    let max = parseInt(document.getElementById('v-total').value);

    let deck = [];
    const bonusList = [
        {id:'c-b', type:'b', icon:'💣', t:'БУМ!', d:`Минус ${state.bP}`},
        {id:'c-g', type:'g', icon:'💎', t:'АЛМАЗ!', d:'+50 очков'},
        {id:'c-r', type:'r', icon:'🚀', t:'РАКЕТА!', d:'Твой ход снова!'},
        {id:'c-l', type:'l', icon:'🎲', t:'УДАЧА!', d:'Рандомные очки'}
    ];

    // Добавляем бонусы, пока не превысим лимит max
    bonusList.forEach(b => {
        let count = parseInt(document.getElementById(b.id).value) || 0;
        for(let i=0; i<count; i++) {
            if(deck.length < max) deck.push({...b});
        }
    });

    // Добиваем остаток вопросами ровно до числа max
    let qCount = 0;
    while(deck.length < max) {
        let n = (qCount % 44) + 1;
        deck.push({type:'q', q:`Цепь №${n}?`, a: chains[n-1]});
        qCount++;
    }

    state.deck = deck.sort(() => Math.random() - 0.5);
    document.getElementById('setup').style.display = 'none';
    document.getElementById('game-ui').style.display = 'flex';
    setTimeout(draw, 50);
}

function draw() {
    const parent = document.getElementById('grid-parent');
    const grid = document.getElementById('game-grid');
    const sb = document.getElementById('score-board');
    sb.innerHTML = state.teams.map((n, i) => `<div class="team-box ${i===state.cur?'active':''}"><div style="font-size:10px;">${n}</div><div class="score">${state.scores[i]}</div></div>`).join('');

    const W = parent.clientWidth - 40;
    const H = parent.clientHeight - 40;
    const N = state.deck.length;
    let bS = 0, bC = 1;
    for (let c = 1; c <= N; c++) {
        let r = Math.ceil(N / c);
        let s = Math.min((W - (c-1)*8)/c, (H - (r-1)*8)/r);
        if (s > bS) { bS = s; bC = c; }
    }
    grid.style.gridTemplateColumns = `repeat(${bC}, ${bS}px)`;
    grid.innerHTML = state.deck.map((it, i) => `<div class="tile ${it.type==='used'?'used':''}" style="width:${bS}px; height:${bS}px; font-size:${bS/2.5}px;" onclick="openTile(${i})">${it.type==='used'?'':i+1}</div>`).join('');
}

function openTile(i) {
    window.actIdx = i;
    const it = state.deck[i];
    const m = document.getElementById('modal');
    ['m-rev-btn','m-ans-box','m-ctrl','m-ok-btn'].forEach(id => document.getElementById(id).style.display = 'none');
    document.getElementById('m-icon').innerText = it.icon || '❓';
    document.getElementById('m-text').innerText = it.q || it.t + " " + (it.d || '');
    if(it.type==='q') {
        document.getElementById('m-rev-btn').style.display = 'block';
        document.getElementById('m-ans-text').innerText = it.a;
    } else {
        document.getElementById('m-ok-btn').style.display = 'block';
        if(it.type==='b') state.scores[state.cur] -= state.bP;
        if(it.type==='g') state.scores[state.cur] += 50;
        if(it.type==='r') state.extra = true;
        if(it.type==='l') {
            let rnd = Math.floor(Math.random() * 61) - 20;
            state.scores[state.cur] += rnd;
            document.getElementById('m-text').innerText = `ТЕБЕ ВЫПАЛО: ${rnd >= 0 ? '+' : ''}${rnd} ОЧКОВ`;
        }
    }
    m.style.display = 'flex';
}

function showAns() {
    document.getElementById('m-rev-btn').style.display = 'none';
    document.getElementById('m-ans-box').style.display = 'block';
    document.getElementById('m-ctrl').style.display = 'flex';
}

function endTurn(win) {
    if(win) state.scores[state.cur] += state.wP;
    nextPlayer();
}

function nextPlayer() {
    document.getElementById('modal').style.display = 'none';
    state.deck[window.actIdx].type = 'used';
    if(!state.extra) state.cur = (state.cur + 1) % state.teams.length;
    state.extra = false;
    draw();
}
</script>
</body>
</html>
