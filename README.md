[index.html.txt](https://github.com/user-attachments/files/24991091/index.html.txt)
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Биржа Труда</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        :root {
            --bg-black: #0f0f0f;
            --card-bg: #1c1c1e;
            --text-white: #ffffff;
            --text-gray: #8e8e93;
            --accent-green: #2ecc71;
            --accent-yellow: #f1c40f;
            --button-green: #1a3d24;
            --button-yellow: #f3d456;
        }

        body {
            background-color: var(--bg-black);
            color: var(--text-white);
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            margin: 0;
            padding: 16px;
            padding-bottom: 100px;
            user-select: none;
        }

        /* Шапка */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .user-info {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: #333;
        }

        .balance-badge {
            background: var(--card-bg);
            padding: 8px 16px;
            border-radius: 12px;
            color: var(--accent-green);
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 5px;
            border: 1px solid #333;
        }

        /* Табы управления */
        .nav-buttons {
            display: grid;
            grid-template-columns: 1.2fr 1.2fr 0.5fr;
            gap: 10px;
            margin-bottom: 25px;
        }

        .nav-btn {
            padding: 15px;
            border-radius: 25px;
            border: none;
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            font-size: 14px;
        }

        .btn-withdraw { background-color: var(--button-green); color: white; }
        .btn-tasks { background-color: var(--button-yellow); color: black; }
        .btn-menu { background-color: #f0f0f0; color: black; border-radius: 50%; width: 50px; height: 50px;}

        /* Секция заданий */
        .section-title {
            font-size: 18px;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .task-card {
            background-color: var(--card-bg);
            border-radius: 20px;
            padding: 15px;
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            gap: 15px;
            position: relative;
        }

        .task-icon {
            width: 60px;
            height: 60px;
            border-radius: 15px;
            object-fit: cover;
        }

        .task-info { flex-grow: 1; }

        .task-name { font-weight: bold; font-size: 15px; margin-bottom: 4px; }
        
        .task-reward {
            position: absolute;
            top: 15px;
            right: 15px;
            color: var(--accent-green);
            font-weight: bold;
        }

        .progress-container {
            width: 100%;
            background: #333;
            height: 6px;
            border-radius: 3px;
            margin-top: 8px;
            overflow: hidden;
        }

        .progress-bar {
            height: 100%;
            background: var(--accent-green);
        }

        .task-btn {
            background-color: var(--button-yellow);
            color: black;
            border: none;
            padding: 8px 15px;
            border-radius: 15px;
            font-weight: bold;
            font-size: 13px;
        }

        .task-btn.done {
            background-color: #d1f7e1;
color: #1a3d24;
        }

        /* VIP Метка */
        .vip-tag {
            background: orange;
            color: white;
            font-size: 10px;
            padding: 2px 6px;
            border-radius: 5px;
            position: absolute;
            top: -5px;
            left: 15px;
        }

        .vip-border { border: 1px solid orange; }

        /* Всплывающее окно (Modal) */
        .modal-overlay {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.8);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        .modal {
            background: var(--card-bg);
            padding: 30px;
            border-radius: 20px;
            text-align: center;
            width: 80%;
            max-width: 300px;
        }

        .modal button {
            margin-top: 20px;
            background: var(--accent-yellow);
            border: none;
            padding: 10px 25px;
            border-radius: 10px;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <div class="header">
        <div class="user-info">
            <div class="avatar"></div>
            <div>
                <div style="font-size: 12px; color: var(--text-gray);">Биржа труда</div>
                <div style="font-weight: bold;">Привет, 👋</div>
            </div>
        </div>
        <div class="balance-badge">
            💳 350 ₽
        </div>
    </div>

    <div class="nav-buttons">
        <button class="nav-btn btn-withdraw" onclick="showPopup()">» Вывод денег</button>
        <button class="nav-btn btn-tasks">↓ Задания</button>
        <button class="nav-btn btn-menu" onclick="showPopup()">⠿</button>
    </div>

    <div class="section-title">📋 Доступные задания</div>

    <div id="task-list">
        <div class="task-card" onclick="showPopup()">
            <img src="https://via.placeholder.com/60/3498db/fff?text=L" class="task-icon">
            <div class="task-info">
                <div class="task-name">Поставить лайки</div>
                <div style="font-size: 12px; color: var(--text-gray);">Выполнено: 1 из 1</div>
                <div class="progress-container"><div class="progress-bar" style="width: 100%;"></div></div>
            </div>
            <div class="task-reward">150 ₽</div>
            <button class="task-btn done">Выполнено</button>
        </div>

        <div class="task-card" onclick="showPopup()">
            <img src="https://via.placeholder.com/60/e74c3c/fff?text=S" class="task-icon">
            <div class="task-info">
                <div class="task-name">Подписаться</div>
                <div style="font-size: 12px; color: var(--text-gray);">Выполнено: 1 из 2</div>
                <div class="progress-container"><div class="progress-bar" style="width: 50%;"></div></div>
            </div>
            <div class="task-reward">350 ₽</div>
            <button class="task-btn">Выполнить</button>
        </div>

        <div class="task-card vip-border" onclick="showPopup()">
            <div class="vip-tag">VIP</div>
            <img src="https://via.placeholder.com/60/f1c40f/fff?text=F" class="task-icon">
            <div class="task-info">
                <div class="task-name">Пригласить друзей</div>
                <div style="font-size: 12px; color: var(--text-gray);">Выполнено: 13 из 50</div>
                <div class="progress-container"><div class="progress-bar" style="width: 26%; background: orange;"></div></div>
            </div>
            <div class="task-reward" style="color: orange;">12 300 ₽</div>
            <button class="task-btn" style="background: orange; color: white;">Выполнить</button>
        </div>
    </div>

    <div class="modal-overlay" id="overlay">
        <div class="modal">
            <h2 style="margin-top: 0;">Упс!</h2>
            <p>Задание не доступно</p>
            <button onclick="closePopup()">Понятно</button>
        </div>
    </div>

    <script>
const tg = window.Telegram.WebApp;
        tg.expand();
        tg.headerColor = "#0f0f0f";

        function showPopup() {
            // Используем нативное окно Telegram, если доступно, иначе обычное
            if (tg.showPopup) {
                tg.showPopup({
                    title: 'Внимание',
                    message: 'Задание не доступно',
                    buttons: [{type: 'close'}]
                });
            } else {
                document.getElementById('overlay').style.display = 'flex';
            }
        }

        function closePopup() {
            document.getElementById('overlay').style.display = 'none';
        }
    </script>
</body>
</html>
