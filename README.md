<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>14 периодов • выбор темы</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #0f172a, #1e293b);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .hidden {
            display: none !important;
        }

        /* ОСНОВНОЙ КОНТЕЙНЕР */
        .container {
            width: 100%;
            max-width: 900px;
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 48px;
            padding: 40px;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
        }

        /* МЕНЮ ТЕМ */
        .menu-header {
            text-align: center;
            margin-bottom: 40px;
        }

        .menu-header h1 {
            font-size: 48px;
            color: white;
            margin-bottom: 8px;
        }

        .menu-header p {
            color: #94a3b8;
            font-size: 18px;
        }

        .themes-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 20px;
            margin-bottom: 30px;
        }

        .theme-card {
            background: rgba(255, 255, 255, 0.03);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 32px;
            padding: 32px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .theme-card:hover {
            background: rgba(255, 255, 255, 0.08);
            border-color: #3b82f6;
            transform: translateY(-4px);
        }

        .theme-icon {
            font-size: 48px;
            margin-bottom: 16px;
        }

        .theme-name {
            font-size: 24px;
            font-weight: 600;
            color: white;
            margin-bottom: 8px;
        }

        .theme-desc {
            color: #94a3b8;
            font-size: 14px;
        }

        .player-input {
            background: rgba(255, 255, 255, 0.03);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 40px;
            padding: 20px 30px;
            display: flex;
            gap: 16px;
        }

        .player-input input {
            flex: 1;
            background: transparent;
            border: none;
            color: white;
            font-size: 16px;
            outline: none;
        }

        .player-input input::placeholder {
            color: #64748b;
        }

        .player-input button {
            background: #3b82f6;
            border: none;
            color: white;
            padding: 12px 32px;
            border-radius: 40px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: 0.2s;
        }

        .player-input button:hover {
            background: #2563eb;
        }

        /* ИГРОВОЙ ЭКРАН */
        .game-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .game-title {
            display: flex;
            align-items: center;
            gap: 16px;
        }

        .back-btn {
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: white;
            padding: 10px 20px;
            border-radius: 40px;
            font-size: 14px;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .back-btn:hover {
            background: rgba(255, 255, 255, 0.15);
        }

        .stats {
            display: flex;
            gap: 24px;
        }

        .stat-item {
            text-align: right;
        }

        .stat-label {
            color: #94a3b8;
            font-size: 12px;
        }

        .stat-value {
            color: white;
            font-size: 20px;
            font-weight: 600;
        }

        /* ПРОГРЕСС */
        .progress-section {
            margin-bottom: 30px;
        }

        .progress-info {
            display: flex;
            justify-content: space-between;
            color: #94a3b8;
            font-size: 14px;
            margin-bottom: 8px;
        }

        .progress-bar {
            height: 6px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 100px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #3b82f6, #8b5cf6);
            width: 0%;
            transition: width 0.3s ease;
        }

        /* КАРТОЧКА ПЕРИОДА */
        .period-card {
            background: rgba(255, 255, 255, 0.03);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 32px;
            padding: 32px;
            margin-bottom: 32px;
        }

        .period-name {
            color: #3b82f6;
            font-size: 14px;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 8px;
        }

        .period-title {
            color: white;
            font-size: 32px;
            font-weight: 600;
            margin-bottom: 8px;
        }

        .period-dates {
            color: #94a3b8;
            font-size: 18px;
        }

        /* ВОПРОС */
        .question-section {
            text-align: center;
            margin: 40px 0;
        }

        .question-text {
            color: white;
            font-size: 24px;
            font-weight: 500;
            line-height: 1.5;
        }

        /* ОТВЕТЫ */
        .answers-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 16px;
            margin: 32px 0;
        }

        .answer-btn {
            background: rgba(255, 255, 255, 0.03);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 24px;
            padding: 24px;
            color: white;
            font-size: 18px;
            cursor: pointer;
            transition: all 0.2s ease;
            text-align: left;
        }

        .answer-btn:hover:not(:disabled) {
            background: rgba(59, 130, 246, 0.2);
            border-color: #3b82f6;
        }

        .answer-btn.correct {
            background: #10b981;
            border-color: #10b981;
        }

        .answer-btn.wrong {
            background: #ef4444;
            border-color: #ef4444;
        }

        .answer-btn:disabled {
            opacity: 0.7;
            cursor: not-allowed;
        }

        /* ПОДСКАЗКА ПРИ ОШИБКЕ */
        .hint-box {
            background: rgba(239, 68, 68, 0.1);
            border: 1px solid #ef4444;
            border-radius: 16px;
            padding: 16px;
            margin: 20px 0;
            color: #fecaca;
            font-size: 16px;
        }

        /* НИЖНЯЯ ПАНЕЛЬ */
        .footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 32px;
        }

        .next-btn {
            background: #3b82f6;
            border: none;
            color: white;
            padding: 16px 48px;
            border-radius: 40px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            transition: 0.2s;
            opacity: 0.5;
            pointer-events: none;
        }

        .next-btn.active {
            opacity: 1;
            pointer-events: all;
        }

        .next-btn.active:hover {
            background: #2563eb;
        }

        .gold-display {
            color: #fbbf24;
            font-size: 20px;
            font-weight: 600;
        }

        /* МОДАЛКА РЕЗУЛЬТАТОВ */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.8);
            backdrop-filter: blur(10px);
            z-index: 1000;
        }

        .result-modal {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: #1e293b;
            border-radius: 48px;
            padding: 48px;
            text-align: center;
            z-index: 1001;
            border: 1px solid rgba(255, 255, 255, 0.1);
            max-width: 400px;
            width: 90%;
        }

        .result-title {
            color: white;
            font-size: 32px;
            margin-bottom: 24px;
        }

        .result-stats {
            color: #94a3b8;
            font-size: 18px;
            line-height: 2;
            margin-bottom: 32px;
        }

        .result-stats span {
            color: #3b82f6;
            font-weight: 600;
        }

        .play-again {
            background: #3b82f6;
            border: none;
            color: white;
            padding: 16px 40px;
            border-radius: 40px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            width: 100%;
        }
    </style>
</head>
<body>

<!-- МЕНЮ ВЫБОРА ТЕМЫ -->
<div id="menu" class="container">
    <div class="menu-header">
        <h1>📚 14 периодов</h1>
        <p>Выбери тему для изучения</p>
    </div>

    <div class="themes-grid">
        <div class="theme-card" onclick="selectTheme('dates')">
            <div class="theme-icon">📅</div>
            <div class="theme-name">ДАТЫ</div>
            <div class="theme-desc">Начало и конец периодов</div>
        </div>

        <div class="theme-card" onclick="selectTheme('duration')">
            <div class="theme-icon">⏳</div>
            <div class="theme-name">ДЛИТЕЛЬНОСТЬ</div>
            <div class="theme-desc">Сколько лет длился период</div>
        </div>
    </div>

    <div class="player-input">
        <input type="text" id="playerNameInput" placeholder="Введи своё имя" value="Историк">
        <button onclick="startGame()">Начать игру</button>
    </div>
</div>

<!-- ИГРОВОЙ ЭКРАН -->
<div id="game" class="container hidden">
    <div class="game-header">
        <div class="game-title">
            <button class="back-btn" onclick="backToMenu()">← Назад</button>
            <span style="color:white; font-weight:600;" id="themeDisplay">Тема: Даты</span>
        </div>
        <div class="stats">
            <div class="stat-item">
                <div class="stat-label">Игрок</div>
                <div class="stat-value" id="playerNameDisplay"></div>
            </div>
            <div class="stat-item">
                <div class="stat-label">Правильно</div>
                <div class="stat-value" id="scoreDisplay">0/14</div>
            </div>
        </div>
    </div>

    <div class="progress-section">
        <div class="progress-info">
            <span>Прогресс</span>
            <span id="counterDisplay">Период 1 из 14</span>
        </div>
        <div class="progress-bar">
            <div class="progress-fill" id="progressBar"></div>
        </div>
    </div>

    <div class="period-card">
        <div class="period-name" id="periodNumber">ПЕРИОД 1</div>
        <div class="period-title" id="periodName"></div>
        <div class="period-dates" id="periodDates"></div>
    </div>

    <div class="question-section">
        <div class="question-text" id="questionText"></div>
    </div>

    <div class="answers-grid" id="answersContainer"></div>

    <!-- Подсказка появляется только при ошибке -->
    <div id="hintBox" class="hint-box hidden"></div>

    <div class="footer">
        <div class="gold-display" id="goldDisplay">💰 5000</div>
        <button class="next-btn" id="nextButton" onclick="nextPeriod()">Следующий →</button>
    </div>
</div>

<!-- МОДАЛКА РЕЗУЛЬТАТОВ -->
<div id="resultOverlay" class="modal-overlay hidden"></div>
<div id="resultModal" class="result-modal hidden">
    <div class="result-title">Тест завершён!</div>
    <div class="result-stats" id="resultStats"></div>
    <button class="play-again" onclick="restartGame()">Ещё раз</button>
</div>

<script>
    // ==================== 14 ПЕРИОДОВ ====================
    const PERIODS = [
        { name: 'ДОПОТОПНЫЙ', start: '4004 до Р.Х.', end: '2348 до Р.Х.', startNum: -4004, endNum: -2348 },
        { name: 'ПОСЛЕПОТОПНЫЙ', start: '2348 до Р.Х.', end: '1921 до Р.Х.', startNum: -2348, endNum: -1921 },
        { name: 'ПАТРИАРХИ', start: '1921 до Р.Х.', end: '1491 до Р.Х.', startNum: -1921, endNum: -1491 },
        { name: 'ПУСТЫНИ', start: '1491 до Р.Х.', end: '1451 до Р.Х.', startNum: -1491, endNum: -1451 },
        { name: 'СУДЕЙ', start: '1451 до Р.Х.', end: '1055 до Р.Х.', startNum: -1451, endNum: -1055 },
        { name: 'ЦАРЕЙ', start: '1055 до Р.Х.', end: '457 до Р.Х.', startNum: -1055, endNum: -457 },
        { name: '70 СЕДЬМИН', start: '457 до Р.Х.', end: '34 по Р.Х.', startNum: -457, endNum: 34 },
        { name: 'ЕФЕС', start: '34 по Р.Х.', end: '100 по Р.Х.', startNum: 34, endNum: 100 },
        { name: 'СМИРНА', start: '100 по Р.Х.', end: '313 по Р.Х.', startNum: 100, endNum: 313 },
        { name: 'ПЕРГАМ', start: '313 по Р.Х.', end: '538 по Р.Х.', startNum: 313, endNum: 538 },
        { name: 'ФИАТИРА', start: '538 по Р.Х.', end: '1517 по Р.Х.', startNum: 538, endNum: 1517 },
        { name: 'САРДИС', start: '1517 по Р.Х.', end: '1798 по Р.Х.', startNum: 1517, endNum: 1798 },
        { name: 'ФИЛАДЕЛЬФИЯ', start: '1798 по Р.Х.', end: '1844 по Р.Х.', startNum: 1798, endNum: 1844 },
        { name: 'ЛАОДИКИЯ', start: '1844 по Р.Х.', end: 'сегодня', startNum: 1844, endNum: 2024 }
    ];

    // ==================== СОСТОЯНИЕ ====================
    let gameState = {
        player: '',
        theme: 'dates', // 'dates' или 'duration'
        gold: 5000,
        correct: 0,
        currentIndex: 0,
        questions: [] // массив вопросов
    };

    let answered = false;
    let currentQuestion = null;

    // ==================== ВЫБОР ТЕМЫ ====================
    function selectTheme(theme) {
        gameState.theme = theme;
        document.getElementById('themeDisplay').innerText = theme === 'dates' ? '📅 Тема: Даты' : '⏳ Тема: Длительность';
    }

    // ==================== НАЧАЛО ИГРЫ ====================
    function startGame() {
        const name = document.getElementById('playerNameInput').value.trim();
        if (!name) {
            alert('Введи имя!');
            return;
        }

        gameState.player = name;
        gameState.currentIndex = 0;
        gameState.correct = 0;
        
        createQuestions();
        
        document.getElementById('menu').classList.add('hidden');
        document.getElementById('game').classList.remove('hidden');
        document.getElementById('playerNameDisplay').innerText = name;
        
        showPeriod();
    }

    // ==================== СОЗДАНИЕ ВОПРОСОВ ====================
    function createQuestions() {
        gameState.questions = [];
        
        PERIODS.forEach((period, index) => {
            const otherPeriods = PERIODS.filter((_, i) => i !== index).sort(() => 0.5 - Math.random());
            
            if (gameState.theme === 'dates') {
                // Для темы "Даты" случайно выбираем: начало или конец
                const type = Math.floor(Math.random() * 2); // 0 - начало, 1 - конец
                
                if (type === 0) {
                    gameState.questions.push({
                        period: period.name,
                        question: `Когда начался ${period.name} период?`,
                        correct: period.start,
                        options: [
                            period.start,
                            otherPeriods[0].start,
                            otherPeriods[1].start,
                            otherPeriods[2].start
                        ].sort(() => Math.random() - 0.5),
                        info: `${period.name}: ${period.start} — ${period.end}`
                    });
                } else {
                    gameState.questions.push({
                        period: period.name,
                        question: `Когда закончился ${period.name} период?`,
                        correct: period.end,
                        options: [
                            period.end,
                            otherPeriods[0].end,
                            otherPeriods[1].end,
                            otherPeriods[2].end
                        ].sort(() => Math.random() - 0.5),
                        info: `${period.name}: ${period.start} — ${period.end}`
                    });
                }
            } else {
                // Для темы "Длительность"
                const duration = Math.abs(period.endNum - period.startNum);
                gameState.questions.push({
                    period: period.name,
                    question: `Сколько лет длился ${period.name} период?`,
                    correct: duration + ' лет',
                    options: [
                        duration + ' лет',
                        (duration + 100) + ' лет',
                        (duration - 50) + ' лет',
                        (duration + 200) + ' лет'
                    ].sort(() => Math.random() - 0.5),
                    info: `${period.name}: ${duration} лет (${period.start} - ${period.end})`
                });
            }
        });
    }

    // ==================== ПОКАЗ ПЕРИОДА ====================
    function showPeriod() {
        if (gameState.currentIndex >= PERIODS.length) {
            showResults();
            return;
        }

        const period = PERIODS[gameState.currentIndex];
        currentQuestion = gameState.questions[gameState.currentIndex];
        answered = false;

        // Информация о периоде
        document.getElementById('periodNumber').innerText = `ПЕРИОД ${gameState.currentIndex + 1}`;
        document.getElementById('periodName').innerText = period.name;
        document.getElementById('periodDates').innerText = `${period.start} — ${period.end}`;
        
        // Вопрос
        document.getElementById('questionText').innerText = currentQuestion.question;
        
        // Прогресс
        document.getElementById('counterDisplay').innerText = `Период ${gameState.currentIndex + 1} из 14`;
        document.getElementById('progressBar').style.width = ((gameState.currentIndex) / 14 * 100) + '%';
        
        // Ответы
        const container = document.getElementById('answersContainer');
        container.innerHTML = '';
        
        currentQuestion.options.forEach((option, index) => {
            const btn = document.createElement('button');
            btn.className = 'answer-btn';
            btn.innerText = option;
            btn.onclick = () => handleAnswer(index);
            container.appendChild(btn);
        });

        // Прячем подсказку и деактивируем кнопку
        document.getElementById('hintBox').classList.add('hidden');
        document.getElementById('nextButton').classList.remove('active');
        document.getElementById('goldDisplay').innerText = `💰 ${gameState.gold}`;
        document.getElementById('scoreDisplay').innerText = `${gameState.correct}/14`;
    }

    // ==================== ОБРАБОТКА ОТВЕТА ====================
    function handleAnswer(selectedIndex) {
        if (answered) return;

        const question = currentQuestion;
        const isCorrect = (question.options[selectedIndex] === question.correct);
        const buttons = document.querySelectorAll('.answer-btn');

        answered = true;

        if (isCorrect) {
            // Правильный ответ
            gameState.correct++;
            gameState.gold += 200;
            
            buttons.forEach((btn, index) => {
                btn.disabled = true;
                if (index === selectedIndex) {
                    btn.classList.add('correct');
                }
            });
            
            document.getElementById('hintBox').classList.add('hidden');
        } else {
            // Неправильный ответ - показываем подсказку
            buttons.forEach((btn, index) => {
                btn.disabled = true;
                if (index === selectedIndex) {
                    btn.classList.add('wrong');
                }
                if (btn.innerText === question.correct) {
                    btn.classList.add('correct');
                }
            });
            
            document.getElementById('hintBox').innerHTML = `❌ Неправильно! Правильный ответ: ${question.correct}`;
            document.getElementById('hintBox').classList.remove('hidden');
        }

        document.getElementById('nextButton').classList.add('active');
        document.getElementById('goldDisplay').innerText = `💰 ${gameState.gold}`;
        document.getElementById('scoreDisplay').innerText = `${gameState.correct}/14`;
    }

    // ==================== СЛЕДУЮЩИЙ ПЕРИОД ====================
    function nextPeriod() {
        if (!answered) return;
        gameState.currentIndex++;
        showPeriod();
    }

    // ==================== РЕЗУЛЬТАТЫ ====================
    function showResults() {
        document.getElementById('resultStats').innerHTML = `
            ✅ Правильных ответов: <span>${gameState.correct} из 14</span><br>
            💰 Всего золота: <span>${gameState.gold}</span>
        `;
        
        document.getElementById('resultOverlay').classList.remove('hidden');
        document.getElementById('resultModal').classList.remove('hidden');
    }

    // ==================== ПЕРЕЗАПУСК ====================
    function restartGame() {
        gameState.currentIndex = 0;
        gameState.correct = 0;
        createQuestions();
        
        document.getElementById('resultOverlay').classList.add('hidden');
        document.getElementById('resultModal').classList.add('hidden');
        
        showPeriod();
    }

    // ==================== НАЗАД В МЕНЮ ====================
    function backToMenu() {
        document.getElementById('game').classList.add('hidden');
        document.getElementById('menu').classList.remove('hidden');
    }
</script>
</body>
</html>
