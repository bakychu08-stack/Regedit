<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vocka go1.0 - Premium Menu</title>
    <style>
        :root {
            --primary: #00f2ff;
            --secondary: #7000ff;
            --bg: #0a0a0c;
            --card: rgba(255, 255, 255, 0.05);
            --text: #ffffff;
        }

        body {
            margin: 0;
            font-family: 'Segoe UI', sans-serif;
            background-color: var(--bg);
            color: var(--text);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            overflow-x: hidden;
        }

        .container {
            width: 90%;
            max-width: 400px;
            background: var(--card);
            backdrop-filter: blur(15px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            padding: 25px;
            box-shadow: 0 0 30px rgba(0, 242, 255, 0.1);
        }

        h1 {
            text-align: center;
            font-size: 24px;
            background: linear-gradient(90deg, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 20px;
        }

        /* Login Section */
        #login-screen {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        input {
            background: rgba(255, 255, 255, 0.07);
            border: 1px solid rgba(255, 255, 255, 0.2);
            padding: 12px;
            border-radius: 10px;
            color: white;
            outline: none;
        }

        button {
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            border: none;
            padding: 12px;
            border-radius: 10px;
            color: white;
            font-weight: bold;
            cursor: pointer;
            transition: 0.3s;
        }

        button:active { transform: scale(0.95); }

        /* Main Menu Section */
        #main-menu { display: none; }

        .feature-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding: 10px;
            background: rgba(255, 255, 255, 0.03);
            border-radius: 10px;
        }

        /* Toggle Switch */
        .switch {
            position: relative;
            display: inline-block;
            width: 45px;
            height: 22px;
        }

        .switch input { opacity: 0; width: 0; height: 0; }

        .slider {
            position: absolute;
            cursor: pointer;
            top: 0; left: 0; right: 0; bottom: 0;
            background-color: #333;
            transition: .4s;
            border-radius: 34px;
        }

        .slider:before {
            position: absolute;
            content: "";
            height: 16px; width: 16px;
            left: 3px; bottom: 3px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }

        input:checked + .slider { background-color: var(--primary); }
        input:checked + .slider:before { transform: translateX(23px); }

        /* RAM Animation */
        #ram-console {
            font-family: 'Courier New', monospace;
            font-size: 10px;
            color: var(--primary);
            height: 40px;
            overflow: hidden;
            background: black;
            padding: 5px;
            border-radius: 5px;
            margin: 10px 0;
            display: none;
        }

        .radio-group {
            display: flex;
            gap: 10px;
            margin: 15px 0;
        }

        .status-bar {
            text-align: center;
            font-size: 12px;
            color: #888;
            margin-top: 15px;
        }

        /* Admin Panel */
        #admin-panel {
            display: none;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            margin-top: 20px;
            padding-top: 10px;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>VOCKA GO1.0</h1>

    <div id="login-screen">
        <input type="text" id="key-input" placeholder="Nhập Key của bạn...">
        <button onclick="checkKey()">ĐĂNG NHẬP</button>
    </div>

    <div id="main-menu">
        <div class="feature-item">
            <span>Nhẹ Tâm & Fix Rung</span>
            <label class="switch"><input type="checkbox"><span class="slider"></span></label>
        </div>
        <div class="feature-item">
            <span>Giảm Lố & Aimlock</span>
            <label class="switch"><input type="checkbox"><span class="slider"></span></label>
        </div>
        <div class="feature-item">
            <span>Reg File VIP</span>
            <label class="switch"><input type="checkbox"><span class="slider"></span></label>
        </div>
        <div class="feature-item">
            <span>Boost RAM Optimization</span>
            <label class="switch"><input type="checkbox" id="ram-toggle" onchange="toggleRam()"><span class="slider"></span></label>
        </div>

        <div id="ram-console"></div>

        <div class="radio-group">
            <label><input type="radio" name="version" value="thg" checked> FF Thường</label>
            <label><input type="radio" name="version" value="max"> FF Max</label>
        </div>

        <button style="width: 100%;" onclick="activate()">KÍCH HOẠT NGAY</button>
        
        <div class="status-bar" id="expiry-time"></div>

        <div id="admin-panel">
            <h3 style="font-size: 14px; color: var(--primary);">ADMIN PANEL</h3>
            <select id="duration" style="width: 100%; background: #222; color: white; margin-bottom: 10px; padding: 5px;">
                <option value="4 ngày">4 Ngày</option>
                <option value="7 ngày">7 Ngày</option>
                <option value="30 ngày">30 Ngày</option>
                <option value="1 năm">1 Năm</option>
                <option value="Vĩnh viễn">Vĩnh Viễn</option>
            </select>
            <button onclick="generateKey()" style="width: 100%; font-size: 12px;">TẠO KEY MỚI</button>
            <p id="key-output" style="font-size: 10px; word-break: break-all; margin-top: 5px;"></p>
        </div>
    </div>
</div>

<script>
    const ADMIN_KEY = "Taolathgngao";
    
    function checkKey() {
        const input = document.getElementById('key-input').value;
        if (input === ADMIN_KEY) {
            document.getElementById('login-screen').style.display = 'none';
            document.getElementById('main-menu').style.display = 'block';
            document.getElementById('admin-panel').style.display = 'block';
            alert("Chào mừng Admin!");
        } else if (input.includes("VOCKA-")) {
            document.getElementById('login-screen').style.display = 'none';
            document.getElementById('main-menu').style.display = 'block';
            alert("Đăng nhập thành công!");
        } else {
            alert("Key không hợp lệ!");
        }
    }

    function toggleRam() {
        const consoleBox = document.getElementById('ram-console');
        if (document.getElementById('ram-toggle').checked) {
            consoleBox.style.display = 'block';
            let i = 0;
            const lines = [
                "> Scanning system memory...",
                "> Cleaning cache: 402MB",
                "> Optimizing heap size...",
                "> Virtual RAM expanded +2GB",
                "> System Stabilized."
            ];
            setInterval(() => {
                consoleBox.innerHTML += lines[i % lines.length] + "<br>";
                consoleBox.scrollTop = consoleBox.scrollHeight;
                i++;
            }, 800);
        } else {
            consoleBox.style.display = 'none';
            consoleBox.innerHTML = "";
        }
    }

    function activate() {
        const btn = event.target;
        btn.innerHTML = "ĐANG XỬ LÝ...";
        setTimeout(() => {
            btn.innerHTML = "ĐÃ KÍCH HOẠT (2 GIỜ)";
            document.getElementById('expiry-time').innerHTML = "Thời gian hiệu lực: 02:00:00";
            alert("Kích hoạt thành công! Hiệu lực trong 2 giờ.");
        }, 2000);
    }

    function generateKey() {
        const duration = document.getElementById('duration').value;
        const randomStr = Math.random().toString(36).substring(2, 8).toUpperCase();
        const newKey = `VOCKA-${randomStr}-${duration.replace(" ", "")}`;
        document.getElementById('key-output').innerText = "Key đã tạo: " + newKey;
    }
</script>

</body>
</html>