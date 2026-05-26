<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stopwatch</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@300;500;700&display=swap');

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            /* Light Pastel Rainbow Aesthetic Background */
            background: linear-gradient(120deg, #ffb3ba, #ffdfba, #ffffba, #baffc9, #bae1ff, #e1baff, #ffb3ba);
            background-size: 400% 400%;
            animation: gradientBG 8s ease infinite;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Roboto Mono', monospace;
        }

        @keyframes gradientBG {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        .container {
            background: rgba(255, 255, 255, 0.35);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border-radius: 30px;
            padding: 40px 50px;
            text-align: center;
            border: 2px solid rgba(255, 255, 255, 0.6);
            box-shadow: 0 25px 50px rgba(149, 117, 205, 0.25);
        }

        .timer-wrapper {
            position: relative;
            display: inline-block;
            margin-bottom: 30px;
        }

        /* Black Zigzag Border Effect */
        .timer {
            font-size: 55px;
            color:red;
            letter-spacing: 3px;
            
            /* Timer Box Styling */
            border: 2px solid #222;
            border-radius: 15px;
            padding: 25px 35px;
            background: rgba(255, 255, 255, 0.3);
            box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.1);
            
            /* Centering and Sizing */
            display: block;
            min-width: 380px;
            position: relative;
            z-index: 1;
        }

        /* Zigzag Top Border */
        .timer::before {
            content: '';
            position: absolute;
            top: -12px;
            left: 0;
            right: 0;
            height: 12px;
            background: 
                linear-gradient(135deg, #222 50%, transparent 50%),
                linear-gradient(225deg, #222 50%, transparent 50%);
            background-size: 20px 12px;
            background-repeat: repeat-x;
            background-position: 0 0;
        }

        /* Zigzag Bottom Border */
        .timer::after {
            content: '';
            position: absolute;
            bottom: -12px;
            left: 0;
            right: 0;
            height: 12px;
            background: 
                linear-gradient(45deg, #222 50%, transparent 50%),
                linear-gradient(315deg, #222 50%, transparent 50%);
            background-size: 20px 12px;
            background-repeat: repeat-x;
            background-position: 0 0;
        }

        /* Vertical Zigzag Lines */
        .zigzag-left {
            position: absolute;
            left: -10px;
            top: 0;
            bottom: 0;
            width: 10px;
            background: 
                linear-gradient(135deg, transparent 50%, #222 50%),
                linear-gradient(45deg, transparent 50%, #222 50%);
            background-size: 10px 20px;
            background-repeat: repeat-y;
        }

        .zigzag-right {
            position: absolute;
            right: -10px;
            top: 0;
            bottom: 0;
            width: 10px;
            background: 
                linear-gradient(45deg, transparent 50%, #222 50%),
                linear-gradient(315deg, transparent 50%, #222 50%);
            background-size: 10px 20px;
            background-repeat: repeat-y;
        }

        .buttons {
            display: flex;
            gap: 12px;
            justify-content: center;
            margin-bottom: 25px;
        }

        button {
            padding: 12px 25px;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            font-family: inherit;
            font-size: 14px;
            font-weight: 500;
            text-transform: uppercase;
            letter-spacing: 1px;
            transition: 0.3s;
            color: #555;
        }

        /* Light Pastel Rainbow Button Styles */
        .start { 
            background: linear-gradient(90deg, #ffb3ba, #ffdfba); 
            box-shadow: 0 0 15px rgba(255, 183, 178, 0.4); 
        }
        .start:hover { 
            box-shadow: 0 0 25px rgba(255, 183, 178, 0.7); 
            transform: scale(1.05); 
        }

        .stop { 
            background: linear-gradient(90deg, #baffc9, #bae1ff); 
            box-shadow: 0 0 15px rgba(186, 255, 201, 0.4); 
        }
        .stop:hover { 
            box-shadow: 0 0 25px rgba(186, 225, 255, 0.7); 
            transform: scale(1.05); 
        }

        .reset { 
            background: linear-gradient(90deg, #ffffba, #ffdfba); 
            box-shadow: 0 0 15px rgba(255, 255, 186, 0.4); 
        }
        
        .lap { 
            background: linear-gradient(90deg, #bae1ff, #e1baff); 
            box-shadow: 0 0 15px rgba(186, 225, 255, 0.4); 
        }
        .lap:hover { 
            box-shadow: 0 0 25px rgba(225, 186, 255, 0.7); 
            transform: scale(1.05); 
        }

        .lap-list {
            background: rgba(255, 255, 255, 0.4);
            border-radius: 15px;
            max-height: 180px;
            overflow-y: auto;
            padding: 10px;
            width: 250px;
        }

        .lap-item {
            color: #1a3a5c;
            padding: 10px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.4);
            font-size: 18px;
            font-weight: 500;
        }

        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-thumb { 
            background: linear-gradient(90deg, #ffb3ba, #ffdfba, #ffffba, #baffc9, #bae1ff, #e1baff); 
            border-radius: 5px; 
        }
        ::-webkit-scrollbar-track { background: rgba(255, 255, 255, 0.2); }
    </style>
</head>
<body>

    <div class="container">
        <div class="timer-wrapper">
            <div class="timer" id="display">00:00:00</div>
        </div>

        <div class="buttons">
            <button class="start" id="startBtn" onclick="toggle()">Start</button>
            <button class="reset" onclick="reset()">Reset</button>
            <button class="lap" onclick="lap()">Lap</button>
        </div>

        <div class="lap-list" id="laps"></div>
    </div>

    <script>
        let startTime = 0;
        let elapsed = 0;
        let running = false;
        let interval;

        const display = document.getElementById('display');
        const startBtn = document.getElementById('startBtn');
        const laps = document.getElementById('laps');

        function toggle() {
            if (running) {
                clearInterval(interval);
                running = false;
                startBtn.textContent = 'Resume';
                startBtn.className = 'start';
            } else {
                startTime = Date.now() - elapsed;
                interval = setInterval(update, 10);
                running = true;
                startBtn.textContent = 'Stop';
                startBtn.className = 'stop';
            }
        }

        function reset() {
            clearInterval(interval);
            running = false;
            elapsed = 0;
            display.textContent = '00:00:00';
            startBtn.textContent = 'Start';
            startBtn.className = 'start';
            laps.innerHTML = '';
        }

        function lap() {
            if (running) {
                const div = document.createElement('div');
                div.className = 'lap-item';
                div.textContent = display.textContent;
                laps.insertBefore(div, laps.firstChild);
            }
        }

        function update() {
            elapsed = Date.now() - startTime;
            display.textContent = format(elapsed);
        }

        function format(ms) {
            let min = Math.floor(ms / 60000);
            let sec = Math.floor((ms % 60000) / 1000);
            let ms10 = Math.floor((ms % 1000) / 10);
            return `${pad(min)}:${pad(sec)}:${pad(ms10)}`;
        }

        function pad(n) {
            return n < 10 ? '0' + n : n;
        }
    </script>

</body>
</html>
